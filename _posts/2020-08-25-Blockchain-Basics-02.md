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

## A Simple Chain But Quite Unbreakable

Each block contains the hash of the block before it, and the next block will contain a hash of the current block. This sequential reference of hashes between blocks is what makes a blockchain useful.

![_config.yml]({{ site.baseurl }}/images/blockchain/blockchain.png)

These linked hashes form the chain, and in doing so they ensure integrity of each individual block. For example, if I were to modify the Data in block 73 without being detected I'd need to do one of two things:

1. Modify the data in such a way that the resulting hash is still the same so the block 74's hash is still valid, better known as a hash collision

2. Modify block 74 to have the updated hash, but now I need to worry about the hash in block 75, then 76, 78, 79, etc.

Assuming that finding a hash collision is impossibly difficult, an attacker would need to modify even block following the block they tampered with. This is feasible _if_ the attacker is operating on the only copy of those blocks, but in most cases a blockchain is **decentralized**.

## Power To The People

A blockchain is decentralized if there are many different locations with their own copies of the chain and those locations are controlled by different groups. These locations are called **nodes** and they each communicate with a other nodes in a peer-to-peer network. Typically, one node will only communicate with a few other nodes and not all of the peer-to-peer network.

![_config.yml]({{ site.baseurl }}/images/blockchain/peer-to-peer.png)

Simply by storing copies of the most recent blocks on other nodes, it becomes much harder to tamper with the blockchain becasue you'd need to make the same changes in the copies on every other node or else the discrepency will be noticed when nodes compare their blocks. So it is certainly hard to retroactively change existing blocks, but what if you tampered with the newest block instead? In most blockchain applications, this is prevented by **consensus**.

When a node wants to add a new block to the chain, it simply needs to put the necessary Data, Time Stamp, and Previous Hash together and add the next Index value. It then sends that new block to all the nodes it communicates with. These nodes receive the new block and then analyze it.

1. Is the Previous Hash correct?
2. Is the Index correct?
3. Is the Time Stamp valid?
4. Is the Data valid according to the blockchain's rules?

If all these checks pass the node adds the new block to its local copy of the blockchain and sends this new block to every other node it communicates with in the peer-to-peer network. If there's no conflicts, it won't take long for the new block to **propagate** across the whole network. If a block shouldn't pass those checks, a node should reject it and simply not save it and not send it to its own peers.

![_config.yml]({{ site.baseurl }}/images/blockchain/propagation.gif)

Assuming the majority of nodes perform these checks honestly, an attacker's invalid blocks won't be able to propagate across the network beyond the node the attacker has access to. The one weakness in the consensus scheme is what's called a **51% attack**, where an attacker gains control over a simple majority (i.e. 51% or more) of the nodes on the network. In this scenario, the attacker can create artificial consensus on new and even past blocks and the integrity of the chain is lost.

