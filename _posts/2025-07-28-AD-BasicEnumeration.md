---
title: "AD: Basic Enumeration"
date: 2025-07-28
categories: [TryHackMe]
tags: [info, vm,  active directory, nmap]
excerpt: "AD: Basic Enumeration TryHackMe"
---

## Introduction

This post provides a comprehensive walkthrough of the TryHackMe room "AD: Basic Enumeration," which introduces fundamental concepts of Microsoft's Active Directory service. This room covers essential enumeration techniques used in penetration testing and red team exercises to gather information about Active Directory environments.


## Task 2: Mapping out the network

What is the domain name of our target?

To identify the domain name, I use Nmap to perform service version detection and OS fingerprinting:
```bash 
sudo nmap -sV -O 10.211.11.10
```
{: file="Nmap command for domain discovery"}

This command reveals service banners and operating system information, which includes the domain name in the output.
> **Answer:** `tryhackme.loc`


What version of Windows Server is running on the DC?
The same Nmap scan provides operating system detection results, showing the specific Windows Server version running on the domain controller.
> **Answer:** `Windows Server 2019 Datacenter`


## Task 3: Network Enumeration With SMB

What is the flag hidden in one of the shares?

The target VM allows anonymous, unauthenticated enumeration, enabling me to gather significant information about the environment. I'll use enum4linux to enumerate SMB shares and other domain information.

First, I enumerate all available SMB (Server Message Block) shares and domain information:

```bash 
enum4linux 10.211.11.10
```
{: file="enum4linux command for comprehensive SMB enumeration"}

This command reveals several accessible shares. Let me examine each one:

```bash 
smbclient \\\\10.211.11.10\\SharedFiles -N
```
{: file="Connecting to SharedFiles share anonymously"}
This share contains a text file with a USB-related story but no flag.

Trying the UserBackups:
```bash 
smbclient \\\\10.211.11.10\\UserBackups -N
```
{: file="Connecting to UserBackups share anonymously"} 

Using the `ls` command within the SMB session reveals a `flag.txt` file. I download and read this file to get my answer:
> **Answer:** `THM{88_SMB_88}`


## Task 4: Managing Users in AD

What group is the user rduke part of?

To find detailed user information, I save the enum4linux output to a file for easier analysis:
```bash 
enum4linux 10.211.11.10 > DomainEnum.txt  
```
{: file="Saving enumeration results for analysis"}

Examining the output file reveals user group memberships, showing me that rduke belongs to:
> **Answer:** `Domain Users`


What is this user’s full name?

The same enumeration output provides full name information for domain users:
> **Answer:** `Raoul Duke`

Which username is associated with RID 1634?

RID (Relative Identifier) 1634 converts to 0x662 in hexadecimal. Searching through the enumeration results for this RID reveals:
> **Answer:** `katie.thomas`



## Task 5: Password Spraying

What is the minimum password length?

The enum4linux output includes domain password policy information, which I can see specifies:
> **Answer:** `7` <br>

What is the locked account duration?

The domain password policy also includes account lockout duration settings:
> **Answer:** `2 minutes` <br>

Perform password spraying using CrackMapExec. What valid credentials did you find? (format: username:password)

First, I need to enumerate domain users using rpcclient: 
```bash 
rpcclient -U "" 10.211.11.10 -N
```
{: file="Connecting to RPC endpoint anonymously"}

Once I connect successfully, I enumerate users with: `enumdomusers`.

Next, I create a password list based on common corporate password patterns:
```bash 
Password!
Password1
Password1!
P@ssword
Pa55word1
``` 
{: file="Common password list (PasswordsToTry.txt)"}

I save the enumerated usernames to a file (TryHackMeUsers.txt) and perform password spraying using CrackMapExec:
```bash 
crackmapexec smb 10.211.11.10 -u TryHackMeUsers.txt -p PasswordsToTry.txt --continue-on-success
```
{: file="Password spraying attack using CrackMapExec"}

The `--continue-on-success` flag ensures the tool continues testing even after finding valid credentials, allowing me to discover multiple compromised accounts.


The attack reveals valid credentials:

And found `tryhackme.loc\rduke:Password1!`


## Task 6: Conclusion

This room demonstrates the importance of proper Active Directory security configuration. 
My **key takeaways** include:

- Anonymous enumeration can reveal extensive information about AD environments.
- Weak password policies make password spraying attacks highly effective.
- Default configurations often allow unauthorized information gathering.
- SMB shares with anonymous access can expose sensitive data.

Understanding these enumeration techniques has been crucial for my learning in both offensive security and understanding how to defend Active Directory environments.
> **Answer:** `No answer needed`


