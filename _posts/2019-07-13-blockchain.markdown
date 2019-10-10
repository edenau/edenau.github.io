---
title: "️⛓️ Building a Minimal Blockchain in Python"
layout: post
date: 2019-07-13 23:00
published: true
headerImage: false
tag:
- Blockchain
- Python
category: blog
author: edenau
description: "Understanding blockchain by coding"
# jemoji: '<img class="emoji" title=":open_file_folder:" alt=":open_file_folder:" src="https://assets.github.com/images/icons/emoji/unicode/1f5c2.png" height="20" width="20" align="absmiddle">'
---

***This post is now available on <a href="https://towardsdatascience.com/building-a-minimal-blockchain-in-python-4f2e9934101d" target="_blank">Towards Data Science — Medium</a>. Check it out!***

Blockchain is not just about Bitcoins or other cryptocurrencies. It is also used for databases such as health records.

Contrary to a somehow popular belief, blockchain is not about data encryption. In fact, all the data in a blockchain is transparent. What makes it special is that it (to some extent) prevents backdating and data tampering. Let’s implement a minimal blockchain using Python. Here is how I built a minimal blockchain, and codes are available on <a href="https://github.com/edenau/Minimal-Blockchain" target="_blank">Github</a>.

Since this is a minimal implementation of blockchains, there will be no algorithms on any distributed network or Proof of Work.

<div class="breaker"></div> <a id="1"></a>

# Hashing

We want a ‘key’ that can represent a block of data. We want a key that is <b>hard to fake or brute force, but is easy to verify</b>. This is where hashing comes in. Hashing is a function H(x) that satisfies the following properties:

- The same input `x` always produce the same output `H(x)`.
- Different (or even similar) inputs `x` should produce <b><i>entirely</i></b> different outputs `H(x)`.
- Computationally easy to get `H(x)` from input `x`, but intractable to reverse the process, i.e. getting the input `x` from a known hash `H`.

This is how Google stores your ‘password’ without actually storing your password. They store the hash of your password `H(password)` such that they can verify your password by hashing your input and compare. Without going into too much details, we will use SHA-256 algorithm to hash our block.

<div class="breaker"></div> <a id="2"></a>

# A Minimal Block

Let’s make an object class called `MinimalBlock()`. It is initialized by providing an `index`, a `timestamp`, some `data` you want to store, and something called `previous_hash`. The previous hash is the hash (key) of the previous block, and it acts as a pointer such that we know which block is the previous block, and hence how blocks are connected.

{% gist ef9f235e6c5f6267079cd06062ae1270 %}

In other words, `Block[x]` contains index `x`, a timestamp, some data, and the hash of the previous block x-1 `H(Block[x-1])`. Now that this block is complete, it can be hashed to generate `H(Block[x])` as a pointer in the next block.

<div class="breaker"></div> <a id="3"></a>

# A Minimal Chain

Blockchain is essentially a chain of blocks, and the connection is made by storing the hash of the previous block. Therefore, a chain can be implemented using a Python list, and `blocks[i]` representing the {i}th block.

{% gist b5205a4b850b2c7f59374f34983d2593 %}

When we initialize a chain, we automatically assign a 0th block (also known as Genesis block) to the chain with function `get_genesis_block()`. This block marks the start of your chain. Note that `previous_hash` is arbitrary in the Genesis block. Adding a block can be achieved by calling `add_block()`.

<div class="breaker"></div> <a id="4"></a>

# Data Verification

Data integrity is important to databases, and blockchains provide an easy way to verify all the data. In function `verify()`, we check the following:

- Index in `blocks[i]` is `i`, and hence no missing or extra blocks.
- Compute block hash `H(blocks[i])` and cross-check with the recorded hash. Even if a single bit in a block is altered, the computed block hash would be entirely different.
- Verify if `H(blocks[i])` is correctly stored in next block’s `previous_hash`.
- Check if there is any backdating by looking into the timestamps.

<div class="breaker"></div> <a id="5"></a>

# Forking

In some case you might want to branch out of a chain. This is called forking as demonstrated as `fork()` in the code. You would copy a chain (or root of a chain) and then go separate ways. It is vital to use `deepcopy()` in Python since Python list is mutable.

<div class="breaker"></div> <a id="6"></a>

# The Takeaway

Hashing a block creates a unique identifier of a block, and a block’s hash forms part of the next block to establish a link between blocks. Only the same data would create the same hash.

If you want to amend data in the 3rd block, the hash of the 3rd block is changed and the `previous_hash` in the 4th block needs to change as well. `previous_hash` is part of the 4th block, and hence its hash changes as well, and so on.
