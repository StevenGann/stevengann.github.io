---
title: Vibe Coding - Caution and Common Sense
description: A critical look at AI-assisted coding, discussing the importance of human oversight, common pitfalls, and how AI can enhance but not replace developer productivity.
categories: [Projects, Programming]
tags: [AI]
mermaid: true
meta_description: "A critical look at AI-assisted coding, discussing the importance of human oversight, common pitfalls, and how AI can enhance but not replace developer productivity."
keywords: "AI-assisted coding, generative AI, developer productivity, software development, AI risks, code quality, human oversight, Vibe Coding, programming best practices, AI tools, technology trends"
---

After I published my [previous post about Vibe Coding](https://stevengann.com/posts/Vibe-Coding/) I got a decent amount of feedback from friends and colleagues, some of it enthusiastic and some of it skeptical, and a couple people who were outright hostile. To some people, it seemed that I was being blindly optimistic and careless about the potential risks of AI-assisted coding. I stand by my previous statements as being my thoughts at the time, and while I have learned a bit more since then I have no intention of retracting them. I do think it is fair to make a few points about how to use tools in a sensible way.

## I Trust AI About As Far As I Can Throw It

To be absolutely and perfectly clear, I highly advise _**against**_ using AI blindly and accepting its output uncritically. The attitude I adopt is that of a senior developer who has an intern to assist them. Generally, what they come up with is good enough and occaisonally brilliant, and often they're using techniques that are newer than my more dated education provided. But every now and then they'll do something exceptionally dumb that seems obvious to me as common sense because of my experience.

Working with AI is very similar to this. In my experience so far, it pays to review every line of code it generates because while it is usually just fine there's occaisonally something that I can easily improve or something that is completely wrong. During a [recent project](https://stevengann.com/posts/Robot/) I noted that while the AI's code passed all tests and technically fulfilled all the features I'd asked for, it missed the _intent_ of the code it was writing and shoehorned in some tricky logic that made things simpler to implement but useless for my actual application. Anyone who has worked on a project that used Test Driven Development will know that this is a problem that isn't unique to AI.

This isn't even limited to Vibe Coding, I take this attitude with all generative AI. A few years back I was doing a consulting job and my client asked me to write a whitepaper explaining the theory behind using a specific module in what was basically a specific case of closed-loop control so they could provide the whitepaper to their own customers as supporting product documentation. I may have been billing hourly, but I wasn't keen on writing something so tedious so I delegated it to ChatGPT. I gave it a flowchart and a list of key points, then prompted it to write four paragraphs covering those main points and referencing the flowchart, with an emphasis on reliability and functional safety. Its first draft was good, but I asked it to rewrite it to use simpler sentence structures and vocabulary that would translate cleanly to Mandarin Chinese because I knew most of my client's current customers were Chineses. The output was alright and better than I expected, but this was back in the early days of ChatGPT and it was pretty bad at repeating itself. I did a few edits to clean it up, exported it to PDF, and submitted it to my client who was thrilled with the result and I was thrilled to have it done in 10 minutes instead of an hour. The key take-away is that the output still needed a human touch for QA, just like if I'd delegated it to an intern.

## Don't Leave AI Unsupervised

As I mentioned in my first post about Vibe Coding, I tried just letting the AI do whatever it wanted to see what would happen. It worked great until it didn't. When something weird broke that the AI couldn't fix on its own, I waded into a ton of spaghetti code and inconsistent architectural choices that were almost as bad as the last major codebase I inherited from a contractor. Since then, I've been careful to make all the design and architecture decisions myself and let AI fill in the gaps and hammer out boilerplate for me. I am also a stickler about comments and code readability and if anything the AI generates doesn't make sense I will ask it to explain the code and probably rewrite it for clarity, or at least add comments.

It seems that for now, AI is quite good at low-level tasks but struggles to grasp the big picture on a project. Filling in a single class is no problem but building a whole application is more challenging. It will get there eventually but it is pretty clear it didn't have a solid plan on how to get from start to finish. Providing the AI with collateral like design documents, code interfaces, and integration tests goes a long way to guiding it in the right direction, but it isn't a substitute for careful auditing of everything it puts out. Not to belabor the point, this is still the same attitude I take with interns and other junior colleagues. They are likely to get stuck and need assistance, eventually so it make it easier on both of us if I am keeping an eye on their work as they go so I understand how they got themselves into that situation and can even help prevent bigger issues down the line.

## AI is a Force Multiplier, Not a Replacement

I really cannot stress this enough. I have had a number of people, including a few close friends, get quite upset when I talk about generative AI because they see it being used to replace human talent and creativity. I sympathize because we know that this is actually happening, as some short-sighted business types are already bragging to investors and LinkedIn about how they've replaced employees with AI. Programmers, artists, customer support, even lawyers are being replaced with ChatGPT and other AI tools, and occaisonally it ends in disaster that is entertaining to watch but more often it is just the derivitive mediocrity we call "AI slop."

Nvidia is nothing less than the heart of the generative AI boom, and it is hard to think of a more authoritative source. Nvidia CEO Jensen Huang has been holding a series of private meetings[^footnote] with executives from companies all across the industry and giving them effectively the same lecture. He tries to explain to them that AI isn't a replacement for humans, it is a force multiplier. He loves to compare AI to electricity or steam engines, and how they advanced human productivity and capability. 

## Conclusions

I feel like generative AI is history repeating itself. In the early 2000s I studied 3D modeling and CGI as a hobby. At the time, it wasn't taken seriously as "real" art because of the impression it was too easy, too lazy, and anyone could do it. A bit before my time, I know that digital artists were similarly gatekept because drawing on a computer wasn't seen as "real" art either. I think about when computers became popular for business use and some companies certainly did replace human works with computers, but in the long run they ended up employing more people than before. When the first compilers were developed, they absolutely did replace some humans. Humans that would manually write out machine code for programs. But I'd wager most of those programmers adapted to the new paradigm and started writing high-level languages.

For better or worse, generative and agentic AI is here to stay. It is going to have a profound impact on the way programmers, artists, and everyone else does their work. It is going to be misused and abused and come companies are going to make very shortsighted decisions.

It is up to us to use these emerging technologies for a positive impact without losing our common sense.

## Note:

[^footnote]: Being private meetings, they haven't been made public so I am afraid you have to take my word on this. For what it is worth, the meetings were held in a cordoned off portion of the Voyager building and while Nvidia employees weren't allowed anywhere near, Jensen's voice traveled quite well through the quiet building and I listened to his whole lecture on two different occaisons.
