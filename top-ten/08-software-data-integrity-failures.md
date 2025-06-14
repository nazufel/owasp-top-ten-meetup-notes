# A8:2021 - Software and Data Integrity Failures

## Overview
[Software and Data Integrity Failures](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/) relate to code and infrastructure that does not protect against integrity violations. This occurs when applications rely on plugins, libraries, or modules from untrusted sources, repositories, and content delivery networks (CDNs), or when an insecure CI/CD pipeline introduces the potential for unauthorized access, malicious code, or system compromise.

Software and data integrity failures can lead to unauthorized code execution, data tampering, supply chain attacks, and complete system compromise. These failures often result from insufficient verification of software integrity, lack of code signing, insecure update mechanisms, or compromised build and deployment processes.

**Common examples include:**
- Applications that rely upon plugins, libraries, or modules from untrusted sources, repositories, and content delivery networks (CDNs)
- An insecure CI/CD pipeline can introduce the potential for unauthorized access, malicious code, or system compromise
- Auto-update functionality, where updates are downloaded without sufficient integrity verification and applied to the previously trusted application
- Objects or data are encoded or serialized into a structure that an attacker can see and modify is vulnerable to insecure deserialization
- The application relies upon plugins, libraries, or modules from untrusted sources, repositories, and content delivery networks (CDNs)

## Reconnaissance

Using gobuster

```sh
gobuster dir -u http://localhost:3000 -w ./dirlist.txt --exclude-length 80117
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://localhost:3000
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                ./dirlist.txt
[+] Negative Status codes:   404
[+] Exclude Length:          80117
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/api                  (Status: 500) [Size: 3017]
/assets               (Status: 301) [Size: 156] [--> /assets/]
Progress: 10 / 11 (90.91%)
/encryptionkeys       (Status: 200) [Size: 7953]
===============================================================
Finished
===============================================================
```
*Note:* The Juiceshop returns non existing urls with a 200, where gobuster expected a 404. Addming `--exclude-lengeth 80117` was required to filter out 200s for a non existent URL from a 200 with a real URL.

## Exploit

### Impact

#### Classification
