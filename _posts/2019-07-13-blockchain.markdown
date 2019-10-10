---
title: "️🏠 Building a Minimal Blockchain in Python"
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

<div class="breaker"></div> <a id="1"></a>

# A Minimal Block
