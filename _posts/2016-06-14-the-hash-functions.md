---
layout: post
cover: 'assets/images/a_fingerprint.png'
navigation: True
title: The hash functions
date: 2016-06-14 10:30:00
tags: cryptography
subclass: 'post tag-cryptography'
logo: 'assets/images/innovea-tech-white.svg'
author: sebastien
categories: sebastien
disqus: true
---

# What is a hash function ?

A hash function takes any string as input and computes a fixed size fingerprint.

The hashes of two equal messages are equal. If the hashes of two strings are equal, it does not imply that the two strings are equal.

When the hashes of two different strings are equal, it is called a collision.

# What the hash functions are used for ?

The hash functions are used to generate a fingerprint of a string, which is easier to manipulate than the initial string. If the two hashes are different, then the two input strings are different.

For example, if we want to check if two big files are different, it is easier to compare their hashes. If their hashes are different, it means the two files are different.

# What are the properties of a secure hash function (from a cryptographic point of view) ?

The properties of a secure hash function from a cryptographic point of view are the following:

- the function must be collision free, ie nobody can find two different string having the same hash value;
- the function must have the hiding property, ie given a hash, it is infeasible to find the initial string having this hash;
- the function must be puzzle-friendly, ie given a hash, it is infeasible to build a string having the same hash;

For more details, please refer to:
- [wikipedia](https://en.wikipedia.org/wiki/Cryptographic_hash_function),  
- [the excellent following course](https://www.coursera.org/learn/cryptocurrency) from Princeton University

# Examples of hash

We will use the md5 algorithm for the following examples.

Let's create first a file hello-world.txt, with the text "Hello World !!!".

To compute the hash, we execute the following command:

```
openssl dgst -md5 hello-world.txt
```

The result is:

```
MD5(hello-world.txt)= feee1b4f67bef56d54b39b3e5bf5ce85
```

If we change the text in the file hello-world.txt from "Hello World !!!" to "hello World !!!", the hash becomes:

```
MD5(hello-world.txt)= be5d0c1c01d30347d47253ef183bb4a0
```

The md5 algorithm is no more used in cryptography, because it is possible to create collisions.
