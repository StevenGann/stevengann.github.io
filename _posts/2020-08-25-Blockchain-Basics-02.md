---
layout: post
title: Blockchain and Cryptocurrency 101 - Part 2
tags: blockchain cryptocurrency
category: article
---

Blockchain and cryptocurrency are a couple of the biggest buzzwords in technology today. People are rushing to put blockchain into all their products somehow, and everyone and their uncle is pushing the next hot cryptocurrency, half of which aren't even cryptocurrencies.

When you get down to it, these are fairly simple applications of some very complex but established technologies.

# Part 2: A Chain of Blocks

## Writer's Block

A blockchain is, as the name implies, a chain of blocks. These blocks are just specially-constructed data structures that relate to each other in a sequence. A simple blockchain block may look like the following:

{% highlight Csharp %}

public class Block   
{
    public UInt64 Index        { get; set; }
    public DateTime TimeStamp  { get; set; }
    public string PreviousHash { get; set; }    
    public string Data         { get; set; }
}
{% endhighlight %}

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

## Well I Didn't Vote For You

In most cases, more than one node is creating blocks, and because it takes some time for new blocks to propagate across the network it isn't uncommon for a new block to be created with different data but the same Index as a block already propagating across the network. As two valid blocks propagate across the network, eventually one of the blocks will arrive at a node that already has the other block. In this situation, the node will need to decide which block to keep and which to discard, since they cannot coexist. Once it makes a decision, it will send this chosen block to all of its peers. There are a number of potential criteria for deciding, and the criteria should be chosen based on the needs and end application of the blockchain itself. 

Some examples include:

* **Time Stamp** is the simplest criteria, simply taking whichever block has the oldest time stamp and therefore started propagating across the network first. 

* **Data rules** could be applied. Most cryptocurrency blockchains use a "proof of work" scheme to create valid block Data (We'll look at that in Part 3), and this proof of work can usually be ranked, so the "better" block can be chosen.

* **Peer nodes** can be considered. If 4 peers have already accepted block A and 3 peers have accepted block B, the node could choose the more popular block and then send it to the peers that chose the other.

There are pros and cons for each critera, and combinations of criteria can be used to deal with ties or other edge cases. What matters most is that the criteria are clearly defined and every node implements them such that every node will make the same decision if needed.

Once these conflicts are resolved, one block is part of the chain's consensus and the other becomes an **orphan block**. Orphan blocks aren't part of the consensus and therefore not part of the official, canon blockchain. The nodes may or may not keep the orphan blocks on file, depending on the application. A more important consequence is for the node that created that orphan block in the first place, and now their data still isn't on the blockchain. They'll simply need to try again and hope their next block doesn't get orphaned.

To avoid getting their blocks orphaned, nodes can improve their chances by getting their block to propagate as fast as possible across the network. The best way to do this is for the node to connect to as many other nodes as it can. This neatly incentivizes nodes to be strongly interconnected, which is good for th network as a whole.

## That Was Easy

It sounds simple, but there are a couple things that can happen to shake up the smooth growth of a blockchain.

### Rollbacks

Mistakes happen. Some bug allowed invalid Data to pass as valid on your cryptocurrency's blockchain, and now a bunch of people are out a few million dollars with of cryptocurrency. You can fix the bug, probably, but we're all stuck with those blocks, right? Not really.

In cases where you can get the whole network to agree on undoing something, you can organize a **rollback**. It's exactly as it sounds, every node on the network agrees to simply discard every block after a specific point. Sometimes cherry-picked data from later blocks will get re-entered in new blocks, but in many cases that's not possible, especially for cryptocurrencies.

This is a messy, often controversial process, because it depends on every node agreeing on the rollback. When some nodes don't agree, you end up with a fork.

### Forks

In the case of a rollback, software upgrade, rule change, or any other change to a blockchain system, there are often nodes that resist. The reasons are varied, often due to disagreement with changes for either personal advantage or philosophies, but the result is that same: Nodes continue adding blocks to the blockchain that aren't compatible with the newer version, which also keeps adding blocks.

In this situation, called a **hard fork**, you literally have a fork in the blockchain. Every block prior to the change exists in both blockchains, but the blocks after are all different because they are no longer compatible. This usually means the whole peer-to-peer network of nodes is split, with both halves being smaller and weaker as a result. On the other hand, enterprising node operators may often choose to run both versions of the blockchain software and remain operational as a node on both networks.

When blockchain changes are more strategically planned and scheduled far enough in advance, there is often the opportunity for a **soft fork**, where blocks generated by outdated nodes are still recognized and accepted by the updated nodes. This softer transition makes it easier for the whole network to migrate over to the new software and usually avoids a split in the long run.

## So What's It Good For?

So now that you know what a blockchain is, the obvious question is what sort of applications is it really suited for? Clearly, entrepreneurs around the world believe it is suitable for everything from finance to AI to digital thermometers, but is it really? Like any technology, a blockchain's suitability for an application comes down to its strengths and weaknesses.

### Strengths

* A blockchain can be **trustless**. Trusty versus trustless isn't something we've really discussed yet, but it is a big draw to blockchains and especially cryptocurrencies. Because a blockchain can be decentralized and is built on strong cryptography, it can be used record and verify transactions without either side of the transaction needing to trust the other.

* A blockchain is **transparent**. Assuming the blockchain is decentralized, every node can hold a copy of the entire blockchain, and therefore review all the Data in every block.

* A blockchain is **immutable**. Since blocks can't be retroactively changed (with the exception of a rollback) then it can always be trusted that blocks represent the Data as was agreed upon at that time with no attempts to change history.

### Weaknesses

* A blockchain is often **slow**. Compared to a centralized database, propagating a block across the network can be quite slow, taking several seconds or even minutes for the network to reach consensus. Then there's the risk of having the block be orphaned and needing to create a new block.

* A blockchain is **heavy**. A single block can be as small as a few dozen bytes, or as big as it needs to be. Over time, as the blocks continue to add onto the chain, the chain can rapidly balloon to hundreds or thousands of gigabytes. There are mitigation strategies, such as only retaining the most recent blocks, but this sacrifices some of the benefits of the blockchain if too many nodes hold incomplete copies of the chain.

* A blockchain is **rigid**. More specifically, the peer network is rigid. Restructuring or changing the implementation of a centralized database is relatively simple because you only have one location to update. For a peer-to-peer network running a decentralized blockchain you need to coordinate the nodes to all switch to the update at the same time. If any fail to update or just choose not to, you end up with a fork in the blockchain.

* A blockchain is **inefficient**. At its most basic, a decentralized blockchain will require many computers running as nodes and computing hashes to verify blocks. If there's a proof-of-work mechanism or similar, computing resources consumed could be absolutely massive. Compared to a centralized database, which could be hosted on a single node or a VM on a datacenter cluster.

Like any technology, the suitability of a blockchain for your application depends on whether the strengths are worth coping with the weaknesses. Is a trustless database worth being slow? Is the transparency worth the logistical issues with upgrades and rollbacks?

One application that is absolutely ideal for a blockchain is the cryptocurrency.