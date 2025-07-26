---
title: "Encryption - Crypto 101"
date: 2025-07-26
categories: [TryHackMe]
tags: [info, vm, linux, gpg, jhonTheRipper]
excerpt: "Encryption - Crypto 101 Room TryHackMe"
---

# Introduction

This is my resolution of Encryption - Crypto 101 room of TryHackMe.

# Task 9: SSH Authentication

Open my Kali-linux vm and connected to TryHackMe via VPN (see more in [Setup Kali VM for TryHackMe](https://shushizinho.github.io/posts/SetupKali/)).

Dowload the RSA TryHackMe file 

![RSA file](image.png)

To crack the password using John the Ripper I cloned the repo using: 

![alt text](image-1.png)

Using Rockyou to crack it:

![alt text](image-2.png)

As it shows, the password is "delicious".

# Task 10: Explaining Diffie Hellman Key Exchange

Resume: 

# Task 11: PGP, GPG and AES

Unziping the task files:

![alt text](image-3.png)

Then, import the key:

![alt text](image-4.png)

And decrypt the file:

![alt text](image-5.png)

Finally, viewing the file content:

![alt text](image-6.png)





