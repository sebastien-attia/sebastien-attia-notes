---
layout: post
cover: 'assets/images/a_lock_with_a_key.png'
navigation: True
title: Asymmetric cryptography
date: 2016-06-16 10:30:00
tags: cryptography
subclass: 'post tag-cryptography'
logo: 'assets/images/innovea-tech-white.svg'
author: sebastien
categories: sebastien
disqus: true
---

# What is the problem with the symmetric cryptography ?

With the symmetric cryptography, a same password is used to encrypt and decrypt a message. The problem is how to safely exchange this password ? This password has to be exchanged in clear in internet and a third person will be able to intercept this password and then decrypt all the encrypted messages.

# What is the asymmetric cryptography ?

The asymmetric cryptography solves the problem of exchange of the password of the symmetric cryptography.

With the asymmetric cryptography, a private/public key pair is generated. A message encrypted with the public key can be decrypted only by the corresponding private key of the pair. And a message encrypted with the private key can be decrypted only by the corresponding public key of the pair. Even if it seems useless to encrypt a message that everybody can decrypt with the public key of the pair, it is used to verify the digital signature of a message (cf the post related to the digital signature).
Why this technology is widely used on the internet ?

The owner of a key pair can publish the public key on the internet. A message encrypted with this public key, can only be decrypted by the corresponding private key of the owner.

## How to generate a private/public key pair ?

The following example is done with the RSA algorithm.
Generate the private key

We generate first the private key in the file myprivatekey.pem:

```
openssl genrsa -out myprivatekey.pem
```

The private key generated is as following:

```
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAx5B632Js5X2ybt0AN+wxusLFPIAT5T3yeyzLku7ZBOvFPcEp
DHN5x5jprbhpTsr79My7LU//gf8QR5PEc65wegU18CgghY3NotFFiHc1uUr7M2kg
UFtDODpW1zf+8TuUYZYNxnc6bdY8JSrlsn7EWG7rQ7M8mSadUdL9CFN28WxtAvMD
n7IVIx/QB9znsQ9bdwI5GXNoM5TY6AHOk8nqTUj8IjOW+1gLQ/VwSJz9E7XM4ugP
fL+NzxSM6U+83xLwZ8OxE+XRtXxXF3ahjxP4lKvrmsWNIYjskk0OmnkHLJXLOAvo
PqVOoMTLSIHxBtAm3w1ayGPLC9JEIJ11B4XKeQIDAQABAoIBAHYDtuYLaqJ8Jtzw
zIRFpVLwg3s3soxKie7Vmr2VibkjRE00wXWfhFDI2Mfm2j/CQiWOPNKbEFpr39C0
TeSrL9C47CDNWg4gwY6beycseBTPhqXscTOUBLhnp5s2fgliVmkvN446S89Qddj0
+UkJNkulrHMot5lKAJa20vPth9VUYdGhKVwQPD9D+EgLCFE8B81hSX9B4Fjwr8Ui
Cht+X/dYJYqPpkGHrTkqELH65gFQVofBoVpfJWANHghaBX8pIGfzJ6iiL27Jv0pi
X/EKT4YR+KMs3+cQFsbgFFKz/Mq4MsQ0JGE7P5jwC0zjGsWlNS6UcF/GsqZy9Ogv
oBP8c0ECgYEA6lD7RF/r1jJ5Ozqi42mYT/1ltJmtj3SBL/VWd9bifiRUghChDzpy
v9na147gskMJWb3oIhAjnTSSymfDMuZKvFO+9E4UFgEkae79Ld4oJCBWmxbfYcSo
h4RQXFdW70S32V6gXyg4n2hXwOoyEYdPVIZe6YRARm1afxfQB26C5U0CgYEA2gg3
CKP7Puq7FmZsnKbEDqmqUS8+W9UUShT6qW2A6Ol2fgOWYejpbLNFM9jc7FRllnhA
5B+xNgTI89okAJ+5OaVkyG0VPYIHWlTuYQqOIoBsrgpgKh8TgjRiqbxu55bFrgWo
P63SzIVXiBprMgIUV4NlBf4Z1OgzQy1knIZ1s90CgYEAjk8OvBExz86p2HIdWdbZ
HcO9kHlBcv4ENBdiI7iLqKbx+GiXGQObi6+JfR+Wkk2qkSmIoZ+BscmrWWi5oeFC
BK0sLX56Ln8VGY1/kOr7IC3Py7ORifSBkoSmtd6JuxnWOxuAdSqdcRtTKKRUMlcm
tCRD4rlivCNQMh5JRyo0L4UCgYB5Q4xoT9vTOHZplPnffpkYlqDVmnMSXEZ2lYh8
Zx0FbaOrno8rUYFSJbrdhUYKYz5FHAjrV/0V0D978N2JQ0yflS+ikZj4prM0OHyE
mHxJEChh+/9ULgiJqF0fjmAYijDUAu16zVCq05bFafwoyiNKMRgk5xiy45pvSHXm
4JniOQKBgQDoHdvsDYSa5aoVi1NQHV2gz5k8m4nNg98Wk5xiUfq9Un1p8G05ECy9
HrQsIRpS3DMgb4goIJHTHfNEZ00h1PH5P5/43fJ2LfaMJV7EtYxgWTdSonVpSiji
rTk01/Hw+qnxA43/gML9TaLERErSwogwrAm6GfjQ8Ci7m468mYt6kQ==
-----END RSA PRIVATE KEY-----
```

Generate the public key, from the private key

```
openssl rsa -in myprivatekey.pem -outform PEM -pubout -out mypubkey.pem
```

The public key generated is as following:

```
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAx5B632Js5X2ybt0AN+wx
usLFPIAT5T3yeyzLku7ZBOvFPcEpDHN5x5jprbhpTsr79My7LU//gf8QR5PEc65w
egU18CgghY3NotFFiHc1uUr7M2kgUFtDODpW1zf+8TuUYZYNxnc6bdY8JSrlsn7E
WG7rQ7M8mSadUdL9CFN28WxtAvMDn7IVIx/QB9znsQ9bdwI5GXNoM5TY6AHOk8nq
TUj8IjOW+1gLQ/VwSJz9E7XM4ugPfL+NzxSM6U+83xLwZ8OxE+XRtXxXF3ahjxP4
lKvrmsWNIYjskk0OmnkHLJXLOAvoPqVOoMTLSIHxBtAm3w1ayGPLC9JEIJ11B4XK
eQIDAQAB
-----END PUBLIC KEY-----
```

## Encrypt a message with the public key

We create a file hello-world.txt with the text "Hello World !!!".

```
openssl rsautl -encrypt -pubin -inkey mypubkey.pem -in hello-world.txt -out hello-world.enc
```

The file hello-world.enc is the following:

```
f>ÞB¢Uba·7Î^?s^Mâ7;O^U¯ãû0EÅ«|+\T^Oþ\^OÍT¢?1d°åü^]å6í^_4E! ¬HAå^?¯Nðs%ÄÖzÍ^K8Ý¶XÛ±æüf^Y^Lx36¹x3^Vb.DW®¯ÛÞÔ^Eú²H^\Á±µ^?^E.(°¼ë^LÅf^F^]'\ý¥l­gg^]­ì(ÿ»ç2^LÁ^H·%Ñ^PÆÁj0-Åú$ÖV¤2ò}G8b^L[{Â^W4^Hd¥Úþ°^YûEFø?©+:^Q^YJb§ÏæÂÞ7À¾£ ù<à¢Õ$ïFPM^]^E£2YQû:^Wõýì½^Eþµÿ+}
```

Decrypt the encrypted message with the private key

```
openssl rsautl -decrypt -inkey myprivatekey.pem -in hello-world.enc -out hello-world.dec
```

We get the initial text "Hello World !!!" in the file hello-world.dec.
