---
title: Engineering Evolves
description: How the engineering profession has changed over generations, and where I believe it is going next
categories: [Programming, History]
tags: [engineering, history, ai]
---

## The Wheel Has Always Turned

I was recently visiting the Computer History Museum in Mountain View, one of my favorite places in the world. I could spend a lifetime wandering the exhibits, examining the artifacts and reading the lore behind them. I stopped in front of the modest display that acknowledges the contributions of Rear Admiral Grace Hopper, one of my personal heroes, to reflect on her wisdom and accomplishments. Grace Hopper was an amazing person, full of profound insight and bold opinions which have guided me through my career. My personal favorite that I have adopted as a core value is:

> The most damaging phrase in the language is: 'We've always done it this way.'

The Admiral's career saw tremendous change in what computers were and how engineers interacted with them, and by extension a change in what engineers themselves were. At the start of her career computer programming was a bespoke, almost artisanal craft that depended on a thorough and intimate knowledge of the hardware in order to write machine-specific code in the form of cryptic numbers only they and the machine could understand. It was messy, time-consuming, and error-prone. Grace Hopper believed this was less than ideal and in 1952 she developed a program that could convert human-readable text into machine code, the A-0 System—a milestone many histories treat as the first working compiler, though the exact “first” label is debated among historians of computing.

At the time, there was no small amount of pushback against her ideas. A program that writes programs? There were many arguments against early compilers like how they didn't always know the most efficient way of doing things, how an experienced human could write better code, etc. Over time, the value of human-readable programs that could be compiled for different computers without being completely rewritten proved to outweigh the caveats. Over time compilers got smarter, more complete, more optimized, more practical. Today writing Assembly is seen as an academic exercise at best because few developers can write code more efficient than a compiler, and certainly not as quickly.

So there I was, staring at Grace Hopper's portrait above a stack of old COBOL books, reflecting on what it must have been like to be an engineer during this critical inflection point where everything you'd learned was suddenly becoming academic and new, flawed tools were being pushed with the promise they'd get better with time. 

## AI: The Compilers of Ideas

There can be little argument that after that inflection point the field of engineering moved along that trajectory. Programming languages evolved and became more high-level. The compiler paradigm spread beyond programming to other fields of engineering. Analog circuits got simulators like SPICE, where circuits are described as code or netlists, parsed into an internal model, and simulated numerically—analogous in spirit, if not identical in mechanism, to the compile-then-run idea. Digital circuit design evolved similarly with the rise of high-level design languages like VHDL and Verilog that allowed an engineer to define in a few lines of code what a compiler would implement with thousands of logic gates. Mechanical engineers got their own DSLs for simulations and specifying parametric designs in code. Even the arts were affected, with languages like Postscript, HTML, and SVG letting artists implement their visions on computers by typing code.

It seems to me that the rise of AI agents and other AI-driven engineering tools are a natural progression in the same way we came to trust compilers with the low-level details and focus on higher-level logic. Languages we actually work with became more and more high-level. Modern languages like Python are already so semantic that someone with little or no programming knowledge can read through them and follow the basic logic. For most everyday application work, languages like JavaScript and C# let the programmer stay far from the machine—though performance-sensitive code in those ecosystems still wrestles with memory, garbage collection, JIT behavior, and hardware realities when it has to. The only real way to make engineering more high-level is to describe your ideas and requirements in plain speech and give it to a program to convert into machine code. There are a couple of intermediate steps, but ultimately that's what tools like Claude, Codex, and Cursor do. They compile ideas instead of structured languages.

## Perfect Is The Enemy of Good

There are many opponents to the use of AI tools for engineering, and almost as many arguments against them. I've discussed these before [on my blog]({% post_url 2025-04-30-Vibe-Coding %}) so I don't want to go into detail about them here. What struck me that day at the museum was that the arguments Grace Hopper faced against her compiler idea are very similar to many of the arguments against AI: Experienced humans can do it better, it is less than ideal, it isn't complete, etc. In time, many teams came to treat compiler limitations as problems to solve and improve, rather than as a reason to stop experimenting with the idea. A new approach being imperfect is not a reason to dismiss it. All a new approach needs to be worth using is to solve some problem better than the current approach, at least in some scenarios.

AI is imperfect. It hallucinates. It makes mistakes. It produces slop. So can the average junior dev running on 2 hours of sleep and 4 cans of Red Bull. But AI solves one very big problem: Speed. By leaving the grunt work of writing actual code to an AI agent, the engineer is liberated from spending so much time writing code. Reviewing the agent's output and providing feedback a couple times is much, much faster so the engineer can focus on the big picture. Measured in recent years, capability gains in AI tooling have outpaced the single-generation leaps we associate with early compiler adoption—an imperfect comparison, but the direction feels familiar. In the meantime, ignoring an imperfect tool and doing things the hard way is foolish.

## The Wheel Keeps Turning

AI is just the latest revolution in an ever-changing field. Engineering has never been static, and changes have always accelerated. Archimedes' screw, the Roman arch, steel, steam engines, fossil fuels, electricity, plastics, computers, compilers, and now AI. Engineering has always been about using the latest methods and the best tool for the job, whether old or new. Rejecting the new is the antithesis of engineering. We don't need to embrace slop just because it is new, but where we see slop we should see an opportunity to innovate.

In another quote commonly attributed to Rear Admiral Grace Hopper:

> The difficulty lies, not in the new ideas, but in escaping from the old ones.