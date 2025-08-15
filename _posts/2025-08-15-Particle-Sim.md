---
title: Particle Simulation
description: Exploring Data Oriented Design through a high-performance particle simulation. Learn how restructuring data around access patterns rather than objects can unlock 3-5x performance improvements and handle thousands of particles at 60 FPS.
categories: [Projects, Programming]
tags: [performance, simulation, multithreading, optimization]
math: true
mermaid: true
---

## Introduction

I've been fascinated with particle systems for years, mostly because they're one of those things that look simple until you try to make thousands of them run at 60 FPS. Most developers, myself included, start with the obvious object-oriented approach and quickly run into a wall when performance matters. After wrestling with this problem on several projects, I finally decided to take a serious look at Data Oriented Design and see what all the fuss was about. The results were pretty eye-opening.

## The Traditional Object-Oriented Approach vs. Data Oriented Design

When I first started building particle systems, I did what any reasonable developer would do—create a `Particle` class that bundles position, velocity, color, and behavior all together. It makes perfect sense from a code organization standpoint, but it turns out to be a performance disaster:

- **Poor cache locality**: Objects scatter data all over memory, causing cache misses left and right
- **Memory fragmentation**: Individual objects create overhead and memory gaps
- **Difficult parallelization**: Object methods and state make thread safety a nightmare

Data Oriented Design flips this whole approach on its head. Instead of thinking about logical relationships, you organize data based on how it's actually accessed. So instead of creating 5000 particle objects, you create separate arrays for each property:

```csharp
Vector2[] positions = new Vector2[circleCount];
Vector2[] velocities = new Vector2[circleCount];
Color[] colors = new Color[circleCount];
float[] radii = new float[circleCount];
float[] restitutions = new float[circleCount];
```

This structure-of-arrays approach gives you some nice benefits:

1. **Cache efficiency**: Related data sits together in memory
2. **Vectorization opportunities**: Processors can operate on multiple elements at once
3. **Easier parallelization**: No shared object state to worry about
4. **Memory efficiency**: No object overhead or padding waste

## Implementing Multi-threaded Physics Updates

Once you have the data structured properly, leveraging `Parallel.For` to distribute physics calculations across multiple CPU cores becomes straightforward:

```csharp
Parallel.For(0, circleCount, i =>
{
    // Apply gravity
    velocities[i] = new Vector2(velocities[i].X, velocities[i].Y + gravity);

    // Apply air friction
    velocities[i] *= airFriction;

    // Update position
    positions[i] += velocities[i];

    // Handle wall collisions...
});
```

Each thread processes a subset of particles independently, with no shared state or synchronization headaches. This scales beautifully with core count and provides near-linear performance improvements on multi-core systems.

## Optimizing Collision Detection: A Multi-Phase Approach

Collision detection is usually where things start to fall apart in particle simulations. A naive O(n²) approach quickly becomes unusable as particle count increases. I learned this the hard way on a previous project. The implementation I settled on uses several optimization strategies:

### Phase 1: Broad Phase Collision Detection

Before doing any expensive distance calculations, we use Axis-Aligned Bounding Box (AABB) rejection:

```csharp
// Quick AABB rejection
float combinedRadius = radii[i] + radii[j];
if (Math.Abs(positions[i].X - positions[j].X) > combinedRadius ||
    Math.Abs(positions[i].Y - positions[j].Y) > combinedRadius)
{
    continue; // Skip expensive distance calculation
}
```

This simple check eliminates the majority of particle pairs without requiring square root operations, dramatically reducing the computational load.

### Phase 2: Avoiding Expensive Square Root Operations

When we do need to check distances, we compare squared distances instead of actual distances:

```csharp
// Use squared distance to avoid expensive sqrt()
float distanceSquared = Vector2.DistanceSquared(positions[i], positions[j]);
float minDistanceSquared = combinedRadius * combinedRadius;

if (distanceSquared < minDistanceSquared)
{
    // Collision detected
}
```

This optimization alone can provide 10-20% performance improvements in collision-heavy scenarios. It's one of those simple tricks that makes a surprisingly big difference.

### Phase 3: Thread-Safe Collision Collection

We use multithreading for collision detection but collect results in a thread-safe manner:

```csharp
var collisions = new ConcurrentBag<CollisionPair>();

Parallel.For(0, circleCount, i =>
{
    // Collision detection logic...
    if (collision_detected)
    {
        collisions.Add(new CollisionPair(i, j));
    }
});
```

The `ConcurrentBag<T>` provides lock-free collection operations, maintaining performance while ensuring thread safety.

### Phase 4: Sequential Collision Resolution

While we can detect collisions in parallel, resolving them requires careful coordination to avoid race conditions. I handle this with sequential processing:

```csharp
foreach (var collision in collisions)
{
    ResolveCollision(collision.CircleA, collision.CircleB,
                    positions, velocities, radii, restitutions);
}
```

This approach balances performance with correctness, parallelizing the expensive detection phase while keeping the resolution phase simple and bug-free.

## Advanced Physics Features

### Velocity-Dependent Restitution

Real-world physics isn't uniform—high-speed collisions typically lose more energy than gentle bumps. The simulation implements velocity-dependent restitution:

```csharp
static float CalculateVelocityDependentRestitution(float baseRestitution, float impactSpeed)
{
    float velocityFactor = Math.Max(0.5f, 1.0f - impactSpeed * 0.02f);
    return baseRestitution * velocityFactor;
}
```

This creates more realistic collision behavior and prevents particles from gaining infinite energy in chaotic scenarios.

### Interactive Mouse Forces

The simulation includes real-time mouse interaction using the same optimization principles:

```csharp
static void ApplyMouseForce(Vector2 mousePos, float radius, float strength,
                           Vector2[] positions, Vector2[] velocities, float[] radii,
                           int circleCount, bool attract)
{
    float radiusSquared = radius * radius; // Pre-calculate for optimization

    for (int i = 0; i < circleCount; i++)
    {
        // AABB rejection first
        if (Math.Abs(positions[i].X - mousePos.X) > radius ||
            Math.Abs(positions[i].Y - mousePos.Y) > radius)
        {
            continue;
        }

        // Use squared distance
        Vector2 direction = positions[i] - mousePos;
        float distanceSquared = direction.LengthSquared();

        if (distanceSquared < radiusSquared && distanceSquared > 0.01f)
        {
            // Apply force calculations...
        }
    }
}
```

Even interactive features benefit from the same optimization strategies used in the core simulation.

## Performance Monitoring and Analysis

The simulation includes comprehensive performance profiling that breaks down frame time into distinct phases:

- **Input Processing**: Mouse handling and user interaction
- **Physics Updates**: Gravity, friction, and movement calculations
- **Collision Detection**: Multi-threaded collision detection and resolution
- **Rendering**: Drawing particles and UI elements

This granular timing information helps identify bottlenecks and measure the impact of optimizations. In practice, with 5000 particles:

- Traditional approach: ~15-20 FPS
- Data Oriented Design: ~60 FPS consistently
- With optimizations: Stable at 60 FPS even with 10,000+ particles

## Memory Layout Considerations

The choice of data structure has profound implications for performance. By keeping related data contiguous in memory, we maximize cache efficiency:

```csharp
// Hot data accessed every frame together
Vector2[] positions = new Vector2[circleCount];
Vector2[] velocities = new Vector2[circleCount];

// Less frequently accessed data
Color[] colors = new Color[circleCount];
float[] restitutions = new float[circleCount];
```

Modern CPUs can prefetch sequential memory access patterns, making array iterations extremely efficient compared to pointer-chasing through object references.

## Scalability and Future Optimizations

The Data Oriented Design foundation makes several advanced optimizations possible:

1. **SIMD Instructions**: Vector operations can be optimized using SIMD intrinsics
2. **Spatial Partitioning**: Grid-based collision detection for O(n) performance
3. **GPU Compute**: Easy migration to compute shaders for massive parallelization
4. **Memory Pooling**: Eliminate garbage collection pressure entirely

## Lessons Learned

Data Oriented Design isn't just about performance—it's about thinking differently about data structures and algorithms:

1. **Organize data by access patterns**, not logical relationships
2. **Leverage parallelization** wherever possible, but be mindful of dependencies
3. **Optimize common cases** first—broad phase rejection eliminates most work
4. **Measure everything**—performance intuition often proves wrong
5. **Balance complexity with maintainability**—not every optimization is worth the complexity

The particle simulation demonstrates these principles in a practical, visual way. The same concepts apply to game engines, scientific simulations, database systems, and any domain where performance at scale matters.

By restructuring thinking around data rather than objects, you unlock performance levels that simply aren't achievable with traditional approaches. The 3-5x performance improvements in this simulation are typical results when applying Data Oriented Design principles to computationally intensive problems.

Whether you're building the next game engine or optimizing scientific simulations, these patterns provide a solid foundation for achieving the performance characteristics that modern applications demand.