---
layout: post
title: Blockchain and Cryptocurrency 101 - Part 3
tags: blockchain cryptocurrency
category: article
---

What is this magic Internet money everyone's talking about? 

# Part 3: Magic Internet Money

## Disclaimer

Cryptocurrency is a hot and controversial topic. There's a lot of money to be made and a lot of money to be lost. There's a lot of brilliant innovation going on and a lot of scams going around. I am not a financial advisor, nor am I qualified to offer any sort of advice regarding investing, trading, or fiscal policy. This article is intended to provide a technical introduction to cryptocurrencies. I do not endorse or suggest investing in any cryptocurrency.

The information provided here is based on "traditional" cryptocurrencies, such as Bitcoin or Litecoin. Statements made about cryptocurrencies in this writing are generalizations and may not be true for every cryptocurrency.

## Worth The Paper It's Printed On

For cryptocurrency to make any sense at all, it's important to look at traditional currencies. In early societies, trade and barter would often include some sort of token that was more useful for trading than actually using. Shells, jewels, and precious metals were common because they were scarce and convenient to carry and trade. As human cultures grew into larger empires, rulers began to establish specific monies and laws surrounding them. Gold and silver stamped with a specific patter was common, with this pattern signifying that the lump of metal was a specific weight and therefore had a specific value. These coins were the start of modern currency, because the government asserted control over supply and integrity, and eventually the value of the metals went down until the coins were worth more than the metal they were made from.

Eventually, gold and silver coins became impractical and a new system arose. Banks would issue a certificate representing sums of coinage that had been deposited with them, and these certificates could be traded and the new owner could redeem the certificate for the coinage. Over time, it was common to simply transact in these certificates rather than dealing with the actual coins.

Around the time of the American Civil War, there were multiple banks in every town and every bank issued their own bills. Counterfeit bills were rampant, which undermined trust in banks and caused economic instability in communities. To mitigate this, the USA federal government established a federal currency, issuing its own bills to banks and taking responsibility for enforcing regulation and ensuring integrity of the bills. Each bill represented a specific quantity of gold and citizens were able to redeem the bills for gold if they so chose. This practice of backing currency with physical was called the "gold standard".

Over time, for reasons much too complex and political to get into here, the gold standard was abandoned. American currency was declared as having value simply because the USA federal government said so. This concept, where currency has no intrinsic value but has value because the government says so, is called **fiat currency**. The value of a US dollar therefore depends on the continued existance on the US government and the belief by people that the dollar has a specific value. Is this a good system? Perhaps. It's working well enough in every nation in the world. Is it a bad system? Perhaps. It certainly has its problems.

## Currency Without Government

Fiat currency works because a trusted authority, i.e. the government, ensures the integrity of the monetary system and controls the supply to prevent the currency from becoming too easy to obtain and therefore less valuable, at least in theory. The issue is that the authority must be trusted to do a good job of this and sometimes that trust is misplaced. Situations like the hyperinflation of Zimbabwe's fiat currency show what can happen when that authority fails dramatically.

The killer feature of a cryptocurrency is that it is **trustless**. Trust in computing is a complex concept and cryptocurrencies are trustless in several different ways. In the big picture, cryptocurrency is trustless in that the blockchain structure makes the entire system completely transparent, from the code implementation that makes it work to the nodes that provide the infrastructure. How much of a cryptocurrency exists is know to everyone, as well as how much everyone has and how much is being produced. Policy changes in a cryptocurrency's implementation are implemented democratically by nature because if nodes don't agree with the changes they will simply refuse the changes and become a hard fork in the blockchain.

## Transaction Scene

Cryptocurrency is, at its core, a ledger implemented as a blockchain. It lists transactions, decreasing the value of one wallet and increasing the value of another wallet, with some additional logic added for mining and transaction fees. For a typical user, transactions are the main function of cryptocurrency, transfering value in and out of their wallet. A cryptocurrency **wallet** is little more than an asymmetrical key pair where your wallet address is the public key.

To make a transaction, you construct a transaction record which states that you are transfering a specific amount from your wallet to another wallet address. Attached to this record is a cryptographic certificate which has been signed with your wallet's private key. You can then send this record to a node which will verify it, add it to a block, mine the block, and then send the mined block onto the peer-to-peer network of nodes. Because the transaction is crpytographically secure and published on a blockchain, neither side of the transaction can hide anything. No bounced checks, no counterfeit bills, no skipping out on a bill. In this regard, the cryptocurrency is again **trustless**.

## A Mining We Will Go

Mining is a big part of most cryptocurrencies, but it isn't often explained what it really is. As we saw in Part 2, a blockchain block contains a Data payload in addition to a Time Stamp and the previous block's hash. A blockchain block for a cryptocurrency uses that Data region for holding transaction records as well as a chunk of data that is quite meaningless. This meaningless padding data, known as the **nonce**, is very special because mining is the process of finding the right nonce so that the hash of the block fits some specific rules. This is called a **proof of work** scheme, and it ensures that the node that authored this block has invested significant computing power into it and therefore has something to lose if it is rejected by the network.

Generally, the rule for mining is fairly simple such as the hash must start with a specific number of 0's. Because the hash function is impossible to predict without actually calculating it, a node must try thousands or millions of different nonce values when searching for one that meets the criteria. Many cryptocurrencies take this a step further by adding a ramping difficulty to the rule, such as requiring another 0 for every 10,000 blocks. 

## Money Printer Go Brrrrrr

Why would difficulty increase? The answer is block rewards and transaction fees. When a node mines a block, the block includes an extra record specifying a specific quantity of value was added to a wallet controlled by the node's owner. This is value out of thin air, similar to printing cash, except the quantity is codified by the rules of the blockchain and that record gets verified along with everything else in the block before nodes will accept them. Additionally, when creating a transaction record users specify a transaction fee they'll pay, and this fee is added to the miner's reward. Because nodes typically pool transactions in a queue waiting to be packed into blocks for mining, a higher transaction fee incentivizes the node to bump you up in the queue and get your transaction processed sooner.

As difficulty rises, these transaction fees tend to rise due to market forces. Nodes tend to become more aggressive in prioritizing transactions that pay better, and may even ignore transactions below a certain level. This is important, because as difficulty rises the block reward is typically designed to decrease in most cryptocurrencies. Since block rewards are created from nothing, each reward increases the total supply and therefore reduces the value of the currency through inflation. By weaning the block reward down to nothing, nodes become reliant on transaction fees as their incentive and the total supply of cryptocurrency eventually hits a maximum.

## A Crypto For Every Occasion

There's hundreds, maybe thousands of cryptocurrencies on public exchanges, and most of them exist because they change something about other cryptocurrencies. Bitcoin, the original cryptocurrency, has been very successful but definitely has its share of problems and limitations. Some problems these **altcoins** typically address include the following.

* **ASIC Mining:** Bitcoin uses SHA-256 as its cryptographic hash function for mining. This is a very good cryptographic hash algorithm, but there are highly efficient Application Specific Integrated Circuits that can calculate SHA-256 orders of magnitudes faster than CPUs or GPUs. As a result, nodes with access to expensive ASICs have a huge competetive edge over nodes run by individuals or smaller operations, discouraging smaller nodes from participating and making the network smaller as a result. Cryptocurrencies have tried a number of different approaches, typically changing the proof of work to algorithms that are harder to implement in ASICs.

* **Privacy:** Once you know someone's wallet address it's simple to check the blockchain and see exactly how much they have, how much they've spent, and to who. This transparency is generally considered a feature, but many companies and organizations prefer a little privacy. Some users employ mixing services which transfer cryptocurrency into a wallet controlled by the service and then moves it across hundreds of wallets controlled by the service and finally into another wallet controlled by the user. This makes it harder to trace who owns what, but not impossible. Some cryptocurrencies address this using a different record scheme such that the actual wallet addresses are encrypted in a scheme that allows nodes to still verify the transaction without knowing anything more about either party.

* **Advanced Features:** Some cryptocurrencies add new features on top of the basic wallet and transaction functionality. For example, Ethereum adds the ability to put scripted logic into transactions and to define tokens that are effectively new currencies on top of the Ethereum blockchain and using the same peer-to-peer network.

* **Different Applications:** Searching for hashes is a good proof of work, but the work itself is not beneficial to anyone and consumes a whole lot of computing power in the proces. To improve on this, some cryptocurrencies have been replacing the proof of work with work towards a task, ranginging from biochemistry simulations to 3D graphics rendering, allowing users to pay fees to have portions of a complex computation task performaed by every node on the network.

## Conclusions

Blockchain and cryptocurrency are a couple of the biggest buzzwords in technology today. They're extensions of relatively old technologies and concepts, and aren't the most complex technologies you can employ. But in the end, they are like every other technology ever developed: Tools to be used when they are suitable, and only then.