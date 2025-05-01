---
title: Vibe Coding - Brave New World
description: Lessons learned with the hot new methodology
categories: [Projects, Programming]
tags: [AI]
mermaid: true
meta_description: "Discover the future of programming with Vibe Coding. Learn how AI coding assistants like Cursor are transforming software development, with real-world examples and practical tips for developers."
keywords: "vibe coding, AI programming, Cursor IDE, AI coding assistant, software development, programming tools, AI pair programming, developer productivity"
---

# Vibe Coding - Brave New World

_Disclaimer: This is not a paid review or endorsement of any product or service._

It seems like vibe coding is all anyone is talking about lately. Some people are saying it is the future and others are saying it is a disaster and is ruining junior devs. The only way to know for sure was to try it out for myself.

## Enter: Cursor

At a recent interdepartmental presentation at Nvidia, I learned that several teams have adopted [Cursor](https://www.cursor.com/) to assist with developing automation scripts for silicon validation among other things. I was very impressed when they showed a screen recording of cursor in action, and was even more impressed when they shared their learnings on how to provide context and documentation to Cursor's agent to get better results. What sold me on the concept was the fact that they were using Cursor to write scripts in a proprietary domain-specific language that the underlying LLM has never seen before, but the agent was able to pick up what it needed by looking at example code and reading documentation. That was enough for me to get a free trial and give it a whirl.

## Going With the Vibe

When I try out a new tool, I like throwing the worst case scenario at it and seeing what happens. In this case, that worst case scenario was [VulcanAI](https://stevengann.com/VulcanAI/). I had started on VulcanAI some months prior and had reached a blocker. I was having trouble figuring out the right approach to implement the next features and I knew I needed a major refactor which was demotivating. I started with a blank slate and started describing the core principles to Cursor's agent and in an afternoon I had a very basic Discord bot up and running in C#. After two days I had it split apart into a modular system of interfaces and implementations, thoroughly documented, and it had even generated a suite of NUnit integration tests! Fantastic, except the tests were failing. I had it debug the tests for me because I was trying to do as little as possible myself, and eventually it had the tests all passing. The only problem was that the Discord connection wasn't working anymore. After a lot of back and forth, it was clear the AI was going in circles doing and undoing the same changes over and over. I needed to fix it the old fashioned way.

It took two days. Most of that time was spent studying the AI's code to understand what was happening, add more logging, and trace through the logic flow to isolate where things were going wrong. It took that long because the code was a **mess.** It was well written to my eyes, but it was disorganized and inconsistent. I have seen better code from undergrad students. Ultimately, the bug turned out to be a single configuration change in the CSPROJ file that broke compatibility with the UTF encoding in the Discord API. I deleted a single line and everything worked flawlessly.

Now, it has been a few weeks since then and I have been using Cursor daily through all sorts of applications and workflows.

## Conclusion: AI Agents are Flawed

AI agents make mistakes. They do dumb things and fail at common sense. The catch is that this is true of humans too. Where Cursor failed in terms of competency it made up for in speed. I have been having Cursor add comments and other documentation to all my projects, and every now and then it misunderstands how some code is designed and I make some edits to the documentation. Of course, it takes 5 minutes to review and edit that documentation that would have taken me a whole day to write in the first place. A whole day of tedious, demoralizing busy-work, completely avoid thanks to my digital intern that handles that for me now. The same goes for code and to some extent debugging. It isn't perfect but there's a lot of value there. The trick is to understand the limitations and work with them.

## I'm a Project Manager

I have an approach that is making vibe coding exceptionally useful, and it is easy to sum up: I'm a project manager instead of a lone developer. In grad school I took a course on software project management and scoffed at most of the techniques we learned, but over the years I found value in them when working with a team and now I find value in them when working with an AI agent. I am less concerned with the specifics of how a piece of code is implemented and more concerned with the code being clear and easy to understand, implementing the features I need while following best practices, and making sure it is all thoroughly documented. I have also learned to be religious with version control when working with an AI agent because you never know when it'll make some subtle mistake that breaks something and you won't notice until it is too late to guess what caused it.

My first attempt at vibe coding was a bit rough because I told the agent to do a thing and didn't care how it did anything. That's a recipe for disaster as much as it would be if I were to give a single sentence description of a project to an intern and let them run amok. Now, I start with a thorough description of what I am doing, all of my requirements and constraints, and documentation on coding practices I want to adhere to and libraries I want to use. I give the AI agent a roadmap by naming classes and functions and defining the API. In one case, I had a good idea of what I wanted a class to look like but couldn't decide how to implement it. I defined a class interface and then asked Cursor to generate a class that implemented that interface. I told it the name of the class, I told it about a library I wanted it to use, and I gave it a rough description of what the class would be used for, and with that it spat out ~500 lines of code that worked perfectly and did exactly what I needed on the first try.

## Guidelines for Managing AI

Based on my experience so far, I've made a few notes on techniques that may help you get the most out of your vibe coding.

### Start with documentation

If you're starting a new project cold, start by giving the agent a description of the project and ask it to generate a thorough README.md suitable for handing the project off to new developers. Provide it with a list of features and design goals, constraints, what platforms and SDKs should be used, etc. The resulting document will help to guide everything that an agent generates in the future.

Further, make sure that all code gets documented. Markdown documents explaining things are good, but comments are the most effective way of making sure the agent understands what's going on in the code. In existing projects, I ask the AI agent to go through and add thorough comments to the code and then I review those comments for accuracy. Sometimes I have to make some edits, which tells me I am saving it from misunderstanding the code in the future. Generally, it is fine.

### Define the API, let the AI implement it

The AI agent I'm using seems to be especially good at filling in gaps. By defining an API and then asking it to handle implementation, I free myself of the Analysis Paralysis that slows me down when writing code from scratch. I do make a point to review all the code that gets generated and occasionally there's something that needs to be changed. I haven't had a situation where the code is _wrong_, exactly. Cursor has proven very competent at recognizing issues and fixing them before I can even comment on them. The most common change I have to make is stylistic, like moving classes into their own files or following a specific style guideline. This leads to the next point.

### More documentation

AI agents get smarter with context, and documentation is part of that. Comments and markdown docs make it smarter about the project, but documentation about the libraries and SDKs make it smarter about those too. I have given my agent links to the C# .NET naming guidelines, DocFX documentation, Discord API documentation, Arduino reference, and more. The documentation I regularly refer to myself is now at the fingertips of this AI agent and it makes good use of them.

## Concerns About Vibe Coding

> _Vibe coding is a crutch to avoid having to learn programming! It is making people worse programmers!_

Thank you, Mr. Strawman. I'm going to be honest, I sort of agree. Vibe coding is a crutch just like my DIY AI knowledgebase assistant is a crutch to assist with my poor memory, or Microsoft Word was a crutch to assist with my father's dyslexia. Crutches are _good_. Crutches _help people_. Yes, it'd be better if people never needed crutches but in the real world some people do.

Frankly, I am not a great programmer. I am competent. I can write code. I have written code for decades, in a dozen languages, and for twice as many platforms. But at the end of the day programming is a means to an end. I want a computer to do something, I don't especially care how. I already know how to program and make time to learn new techniques and languages for the sake of learning, but I don't want that to stand between me and my goals. By avoiding things like analysis paralysis, the tedium of writing documentation, or the frustration of configuring CI/CD, I can focus on the higher level tasks and keep moving toward my goals.

As for making people worse? That's probably true. Like autocorrect made people worse at spelling, there will certainly be some people who believe they can let the AI do it all while they get a whole app from a single sentence description. They might be right sometimes, and they might get themselves into a mess of unmaintainable, nonsensical, undocumented code other times. New tools and technologies have a tendency of doing that, but we didn't abandon microwaves just because some people never learned decent cooking.

## The Future of Programming with AI

The field of AI is rapidly evolving. A year ago AI code generation was a novelty at best, and now I am convinced it will be as essential to developers as IDEs and realtime linters. The current state of agentic AI makes it comparable to working with humans and it seems they're only getting smarter. I have no idea what's going to happen next.

I have a pretty good idea what won't happen, though. AI agents aren't going to replace programmers any more than compilers did. Like compilers and high level languages, these new tools are going to redefine how programmers operate and empower them to do so much more than they ever have. That's good news for some programmers. The programmers that should be nervous are the ones who don't want to learn the new tool, and won't be able to compete with the productivity of those who do.