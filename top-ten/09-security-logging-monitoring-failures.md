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

```
