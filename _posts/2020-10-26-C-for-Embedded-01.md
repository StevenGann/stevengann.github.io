---
layout: post
title: C for Embedded Systems - Part 1
tags: programming electronics
category: article
---

Embedded development with C is a completely different animal from more common forms of software development.

In this article I'm going to provide a primer of basic concepts needed for C and bare-metal development. There's a lot of variation between applications, but these concepts will smooth out the learning curve so you can seek out more specific resources for your projects. This article assumes you have a basic working knowledge of programming in general, but not necessarily C.

# Part 1: Oh Say Can You C?

## The Latin of Programming

C is commonly called a universal programming language, sometimes **the** universal programming language. When a new CPU architecture is developed, a C compiler is one of the first things developed for it so, in theory, every device should be able to run any program written in C. This is a great promise, since C is also a very simple language to learn! The entire language fits in a rather thin book [[Internet Archive]](https://archive.org/details/the_c_programming_language_2_20181213/mode/2up) and I always say you can learn the whole language is a single weekend.

## The Catch

_A version of_ C will compile and run on pretty much anything if you have a compatible compiler. This qualification is important because not only do different compilers target different versions of C, they often include functionality specific to themselves and not part of the C standard.

### C Versions
	
The most common form of C in the embedded world is the the first version to become widespread in the industry: **C89** also known as **ANSI C**. This is the version of C described in the book linked above, was published in 1989, and as the name suggests is a formal ANSI standard (ANSI X3.159-1989). C89 is implemented by pretty much every microcontroller you can imagine, from Microchip to Texas Instruments.

A second version to be aware of in **C99**. As you'd expect, this version was published in 1999. There was also C90 and C95, but those are much less common today so to keep things simple I'm going to roll their improvements in with C99. Over C89, C99 added

* the `wchar` type, which is a 16-bit character
* digraphs
* macros for things like using `and` instead of `&&`
* the `long long` type, which is 64-bit signed integer
* the `_Bool`, `_Complex`, and `_Imaginary` types
* static array indices
* designated initializers
* compound literals
* variable-length arrays
* flexible array members
* variadic macros
* the `restrict` keyword

...and then C99 removed some features that C89 had, such as implicit function declarations, which honestly you should never do anyway.

If you write C89 with good style, most applications will compile and run fine under C99 too. Because of this, and because C89 support is more common than C99 in embedded systems, we'll be using C89.

### Getting Pragmatic

So your microcontroller's manufacturer provides a compiler that implements C89 at least. That's enough, right? No, it isn't, and this is actually by design. When Brian Kernighan and Dennis Ritchie were designing C, they knew that compilers might need more information than the code itself, so they specified the `#pragma` preprocessor directive. It is often the biggest difference between C compilers, especially for microcontrollers.

The actual useage of `#pragma` is not really specified by the C standard, but is left for the compiler developers to define it however they like. For some microcontrollers, such as the Microchip PICs, the `#pragma` directive is used to load configuration data into the microcontroller while the compiled code is being flashed to its internal memory.

Because `#pragma` is specific to the compiler you're using, and the only way to know how to use it is the compiler's documentation, you're largely on your own. Example code from the manufacturer is often the most helpful source of information on it.

## Actual Code

So let's talk about code. Traditionally, you'd look at a "Hello World" example when learning a language, but many embedded systems don't have a really simple way of displaying text. Instead, we'll assume there's an LED connected to the pin of a microcontroller and we're going to make it blink. I'm not going to go in-depth about standard C89 syntax or features, please refer to the book linked above for information on that.

**Note:** The following example code is specifically for a very specific MSP-430 microcontroller. While the names are different for every manufacturer and microcontroller family, the concepts and techniques demonstrated are used everywhere.

{% highlight c %}
#include <msp430f2274.h>

int main(void)
{
  WDTCTL = WDTPW | WDTHOLD;
  
  P1DIR = 0x41;
  P1OUT = 0X01;
  
  int i;
  
  while(1)
  {
    P1OUT ^=0x41;
	
    for(i=0; i<1000; i++)
    {
      asm(" NOP");
    }
  }

  return 0;
}
{% endhighlight %}

Simple, right? Even if you know C and this is still confusing, that's fine because a lot of this isn't standard C. Let's go over the weird stuff one at a time. 

`#include <msp430f2274.h>`

This is extremely important, and most microcontroller and microprocessor manufacturers do this. As you may know, the `#include` preprocessor directive allows the following code to use functions, variables, constants, or anything else defined in the included file. The use of `< >` angle brackets rather than `" "` quotes also means that you're not supplying the header file, the compiler is expected to know where this specific file is, but why would it?

Most microcontroller manufacturers will include alongside their compiler a huge library of header files just like this. Inside it dozens, possibly hundreds of structs, macros, constants, and maybe even some functions to simplifying working with the microcontroller. This is because interacting with the hardware of a microcontroller, such as toggling an output pin or even configuring that pin as an output in the first place, requires reading and writing either registers or memory-mapped IO. You could certainly go through the datasheet for the chip and figure out the address, or use inline Assembly to do it (more on that later), but that's extremely tedious.

In practice, the register names in the datasheet are _usually_ how they are represented in the header file, so you can use the hardware datasheet as a programming guide for this sort of low-level thing, and if a register name isn't working, you can poke around the header files that came with your compiler and see what it is actually called.

****
### A Note on Headers

Unless you're familiar with C++, header files are a strange concept. The intention is that header (`*.h`) files contain the function prototypes, constants, and macros, while the code files (`*.c`) contain the actual code implementations of functions and structs. So if your you've got a `usefulFunctions.c` file, you have a `usefulFunctions.h` file that's just the function prototypes. Then in other parts of your program you toss in `#include "usefulFunctions.h"` and you're all set. Of course, header files _can_ contain code too, so a lot of times a library will be just a single header file.
****


So, from that one include we've got a good idea of where these other non-C keywords came from.

`WDTCTL = WDTPW | WDTHOLD;`

You might be able to guess that `WDTCTL` is a register name specific to this microcontroller family, and you'd be right. It is the **W**atch**D**og **T**imer **C**on**T**ro**L** register, and is listed in the microcontroller's datasheet. The next two are more complicated. They are bit masks, and this process of combining them is called bit masking.

The objective is to disable the watchdog timer because we're not using it. To do so, we need to set specific bits in the `WDTCTL` register to `1` and others to `0`. We could look up the exact position of the bits we need and then hardcode them as numbers, but the manufacturer provided the bit names as constants representing the needed values. The `|` bitwise OR operating will combine the bits into a single value before writing that value to the register.

```
P1DIR = 0x41;
P1OUT = 0X01;
```

This is pretty much the same thing, except there aren't any handy masks for us this time and we need to do some math.

There's a number of registers associated with the `P1` (port 1) pins on the microcontroller I'm using. In this case, `P1` has 8 pins and they can all be inputs or outputs and I can mix and match them freely, but I need to specifically configure them. The datasheet tells me that setting a bit in the `P1DIR` to `1` will make that pin an output, while `0` will make it an input. I want the pins `P1.0` and `P1.7` to be outputs so I write `1000001` to the P1DIR register, which is actually `0x41`. There is no binary literal in C89, so I have to write the value in hexadecimal.

Noe that those pins are configured as outputs, I can use the `P1OUT` register to set the pin high (`1`) or low (`0`), and for now I just want to set pin `P1.0` high and leave `P1.7` low. To accomplish this I write `00000001` to `P1OUT`, which is obviously just `0x01` in hex.

`P1OUT ^=0x41;`

Now we're geting a little fancy. I want to invert the states of those two pins. There's a lot of different ways to do this but this is one of the most efficient. The `^=` operator is equivalent to the following:

`P1OUT = P1OUT ^ 0x41;`

And the `^` operator is a bitwise `XOR` so the first and last bits will have their values inverted no mater what they are.

Lastly, I need to slow down this code because if it runs as fast as possible then the LED will be blinking too fast for the eye to see. There's several ways to do this, but the most basic is the busy loop. I repeat something meaningless hundreds of times before continuing. In this cas, I repeat this:

`asm(" NOP");`

So what the heck is that? That's embedded Assembly.

## You Got Your C In My Assembly!

In case you didn't know, Assembly is a lower-level language than C. It's typically 1:1 with the machine code the CPU actually uses but a little easier to read, and it has full access to everything the CPU can do, from registers to unique instructions no other CPU has. Because of this, Assembly varies a lot between CPU architectures and can practically be a whole different language.

When you compile a C program, the compiler generates Assembly code equivalent to your C code and passes that to the assembler which generates the final machine code. Before of this, you can use C's `asm` feature to have **inline Assembly** within your C code. This way you don't have to write Assembly, but if there's something Assembly can do that C can't, you can still use those CPU features.

In this case, we're going running a single CPU instruction: `NOP`. This is also called "co operation" and is common across many kinds of CPU. As the name implies, it is an instruction that does nothing, it just consumes a single instruction cycle of the CPU's time. Doing `NOP` 1,000 times actually takes nearly a second on this specific microcontroller, so it makes a nice simple delay. It's not super accurate for something like a clock, and this sort of busy waitign keeps the CPU at full power consumption so this isn't good for running on small batteries either. It'll do for blinking an LED though.