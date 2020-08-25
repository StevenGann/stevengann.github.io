---
layout: post
title: Blockchain and Cryptocurrency 101 - Part 2
---

Blockchain and cryptocurrency are a couple of the biggest buzzwords in technology today. People are rushing to put blockchain into all their products somehow, and everyone and their uncle is pushing the next hot cryptocurrency, half of which aren't even cryptocurrencies.

When you get down to it, these are fairly simple applications of some very complex but established technologies.

# Part 2: A Chain of Blocks

## Writer's Block

A blockchain is, as the name implies, a chain of blocks. These blocks are just specially-constructed data structures that relate to each other in a sequence. A simple blockchain block may look like the following:

```Csharp
public class Block   
{
    public UInt64 Index        { get; set; }
    public DateTime TimeStamp  { get; set; }
    public string PreviousHash { get; set; }    
    public string Data         { get; set; }
}
```

1. A unique index for the block
2. A date and time stamp of when the block was created and added to the chain
3. The hash of the previous block, using a cryptographic hash algorithm such as MD5 or SHA-256
4. The data payload this block is storing

By itself a block isn't a whole lot. In fact, a single block by itself is useless. It gets more interesting once there's a chain of blocks.

## Chain Gang

Each block contains the hash of the block before it, and the next block will contain a hash of the current block. This sequential reference of hashes between blocks is what makes a blockchain useful.

![_config.yml]({{ site.baseurl }}/images/blockchain/blockchain.png)

These linked hashes form the chain, and in doing so they ensure integrity of each individual block. For example, if I were to modify the Data in block 73 without being detected I'd need to do one of two things:

1. Modify the data in such a way that the resulting hash is still the same so the block 74's hash is still valid

2. Modify block 74 to have the updated hash, but now I need to worry about the hash in block 75, then 76, 78, 79, etc.

