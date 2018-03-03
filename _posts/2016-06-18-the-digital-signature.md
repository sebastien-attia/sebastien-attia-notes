---
layout: post
cover: 'assets/images/a_signature.png'
navigation: True
title: The hash functions
date: 2016-06-18 10:30:00
tags: cryptography
subclass: 'post tag-cryptography'
logo: 'assets/images/innovea-tech-white.svg'
author: sebastien
categories: sebastien
---

# What is a digital signature ?

Like a signature, a digital signature allows to authenticate the emitter of the message and verify the message has not been modified since it has been signed.
How does the digital signature work ?

The emitter of the message computes the hash of the message (cf post on hash functions) and encrypts the hash with his private key (cf post on asymmetric cryptography).

To verify the signature, the receiver has to:

1. Decrypt the digital signature with the public key of the emitter; he will then obtain the hash function of the message, computed by the emitter,

2. Compute the hash of the message,

3. Compare the two hashes; if the two hashes are equal, it means the message has not been altered and the message is coming from the owner of the public key he used to decrypt the signature.