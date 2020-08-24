---
layout: post
title: Blockchain and Cryptocurrency 101 Part 1
---

Blockchain and cryptocurrency are a couple of the biggest buzzwords in technology today. People are rushing to put blockchain into all their products somehow, and everyone and their uncle is pushing the next hot cryptocurrency, half of which aren't even cryptocurrencies.

When you get down to it, these are fairly simple applications of some very complex but established technologies.

# Part 1: Cryptography and You

## Hash Slinging Slasher

Hash is delicious. A couple centuries ago, pubs would take all their scraps of meat, cheese, and potato from the previous day, fry it up with some strong seasonings and lots of salt, and serve it up as a cheap hot meal. Eventually, hash became a staple dish on both sides of the Atlantic, not just a way to make leftovers more paletable. Americans might know it best as corned beef hash, very finely-ground meat with diced potatoes and fried up for breakfast.

What does this have to do with cryptography? Hash algorithms are named for the food. When you cook hash, you break down larger foods into small pieces, mix them up, add things, and serve up a portion of it that contains a little of everything but not the whole of anything. A hash algorithm does the same thing, taking some amount of data, breaking it into small pieces, mixing them up in a predictable way, and returning a much smaller piece of data that is influenced by every bit that went in, but doesn't have enough information to reconstruct the original data.

### A Simple Hash

The simplest hash algorithm is probably a checksum. The algorithm is very, very simple:

```Csharp
byte[] data = System.Text.Encoding.ASCII.GetBytes("Very important information!");
int sum = 0;
for (int i = 0; i < data.Length; i++)
{
    sum = sum + data[i];
}
```

Running this code returns a sum of 2699. If a single byte of the input text were to change, that sum would change too. Checksums are great because they are very fast and make it easy to see if there have been any errors in transmitting data. The problem is they aren't very secure and are easy enough to tamper with. Simply rearranging the bytes is enough to fool a checksum, so "inVentory formation! armpit" has the same checksum as the example above. There's a number of ways to improve checksums, but no matter what they're still relatively simple to tamper with, offseting your malicious changes with other changes to ensure the resulting checksum is right.

### Stronger Hashes

This is why cryptographic hash algorithms were created. Cryptographic hash algorithms are usually designed with a few traits in mind.

Computational complexity is the first line of defense for a hash algorithm. If a hash algorithm can't be reversed then an attacker could still use a "brute-force" attack, trying millions or billions are different changes to the tampered data in hopes of finding a combination of changes that result in the same hash. If it takes even a few milliseconds for to calculate the hash using the algorithm, it could take years or even centuries to try enough permutations that a match may be found.

High entropy is important too. In this context, that means that changing a single bit of the input data will dramatically change the resulting has, and if you generate thousands of hashes from data that is _almost_ identical those hahses won't have any patterns in common. This apparently random output makes it impossible to proedict any part of the hash without actually calculating the whole thing.

The algorithms behind these hashes are complex and devised by people who've spent their whole careers studying them. The popular ones have been the subjects of extensive study by experts all over the world, from academia to major governments to malicious hackers. 

A few of the more popular hashes include:

* MD2, MD4, MD5, MD6
* SHA-1, SHA-256, SHA-512
* CRC-16, CRC-32, CRC-64
* PBKDF2
* Scrypt

Depending on your application, and how you need to balance performance versus security, there is almost certainly a standardized hash algorithm suitable for you. If security is important and you do not have a PhD in cryptography, you should use one of the standardized hash algorithms. 

