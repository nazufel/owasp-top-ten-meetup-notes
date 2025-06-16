# A9:2021 - Security Logging and Monitoring Failures

## Overview
[Security Logging and Monitoring Failures](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/) occur when applications fail to adequately log security events, monitor for suspicious activities, or respond to security incidents in a timely manner. This vulnerability category encompasses insufficient logging, inadequate log protection, missing alerting mechanisms, and poor incident response capabilities.

Security logging and monitoring failures can prevent organizations from detecting breaches, understanding attack patterns, and responding effectively to security incidents. Without proper logging and monitoring, attacks may go undetected for extended periods, allowing attackers to maintain persistence, escalate privileges, and exfiltrate sensitive data. These failures often result from inadequate logging practices, lack of centralized log management, insufficient monitoring tools, or poor incident response procedures.

**Common examples include:**
- Auditable events, such as logins, failed logins, and high-value transactions, are not logged
- Warnings and errors generate no, inadequate, or unclear log messages
- Logs of applications and APIs are not monitored for suspicious activity
- Logs are only stored locally
- Appropriate alerting thresholds and response escalation processes are not in place or effective
- Penetration testing and scans by dynamic application security testing (DAST) tools do not trigger alerts
- The application cannot detect, escalate, or alert for active attacks in real-time or near real-time

## Reconasissance
Enumerating possible endpoints and URL paths is part of a normal pentest. Running [gobuster](https://github.com/OJ/gobuster) with a custom wordlist showed an interesting directory to check out.

```sh
gobuster dir -u http://localhost:3000 -w ./dirlist.txt --exclude-length 80117,80118,80119
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://localhost:3000
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                ./dirlist.txt
[+] Negative Status codes:   404
[+] Exclude Length:          80117,80118,80119
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/api                  (Status: 500) [Size: 3017]
/assets               (Status: 301) [Size: 156] [--> /assets/]
/support/logs         (Status: 200) [Size: 8890]
Progress: 10 / 11 (90.91%)
===============================================================
Finished
===============================================================
```
`gobuster` shows a hit on `/support/logs` directory. That can something interesting to check out.

## Exploit
Navigating to `/support/logs` shows a directory of log files. There's an `access.log` file.

```txt

```
This shows what may be nginx logs with logs of our requests but also could contain requests of other users.

## Impact
I