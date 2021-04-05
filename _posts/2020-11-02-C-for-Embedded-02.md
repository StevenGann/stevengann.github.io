---
layout: post
title: C for Embedded Systems - Part 2
tags: programming electronics
category: article
---

Embedded development with C is a completely different animal from more common forms of software development. In this article I'm going to provide a primer of basic concepts needed for C and bare-metal development. There's a lot of variation between applications, but these concepts will smooth out the learning curve so you can seek out more specific resources for your projects. This article assumes you have a basic working knowledge of programming in general, but not necessarily C.

# Part 2: Pardon The Interruption

So you have a C program running on your microcontroller. It's got a main function and a loop so it keep running over and over again. Every time it runs, you check the registers for your peripherals, say a serial UART, an I2C peripheral, SPI, there's a ADC to check, and of course you carefully timed how long this loop takes and you're counting loop iterations so you can do some tasks only after a specific amount of time has passed. But now those tasks are slowing down the main loop and throw off your timing so you need to compensate for that... and it's a big mess very quickly. On an operating system, you might solve this sort of issue with multithreading or some other asynchronous technique. On a microcontroller you need interrupts.

## Interrupts and You

During normal operation, a microcontroller will move linearly from instruction to instruction. When an interrupt in enabled and triggered, the microcontroller jumps to a designated subroutine and then returns when the subroutine is finished. This subroutine is called an Interrupt Service Routine, ISR, and in C is implemented as a normal function with the `interrupt` keyword added to the declaration. Actually setting the function as an ISR varies between compilers and manufacturers, but in most cases a `#pragma` command before the function definition or or an `interrupt [index]` in the declaration will tell the compiler to configure it as an ISR.

**AVR Example:**

{% highlight c %}

interrupt [TIM0_OVF] void timer0_overflow (void)
{
//called automatically on TIMER0 overflow
}

{% endhighlight %}

**MSP-430 Example:**

{% highlight c %}

#pragma vector = TIMER0_A0_VECTOR
interrupt void timer0_A0_irq(void)
{
    P1OUT ^= BIT0;
}

{% endhighlight %}

