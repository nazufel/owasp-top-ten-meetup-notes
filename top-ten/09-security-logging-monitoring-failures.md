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
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:24 +0000] "GET /rest/products/1/reviews HTTP/1.1" 200 521 "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:24 +0000] "GET /rest/products/1/reviews HTTP/1.1" 304 - "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:24 +0000] "GET /rest/products/1/reviews HTTP/1.1" 304 - "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:24 +0000] "GET /rest/products/1/reviews HTTP/1.1" 200 521 "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:24 +0000] "GET /rest/products/1/reviews HTTP/1.1" 200 521 "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:24 +0000] "POST /rest/products/reviews HTTP/1.1" 200 318 "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:30 +0000] "GET /rest/user/whoami HTTP/1.1" 304 - "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:30 +0000] "GET /rest/products/24/reviews HTTP/1.1" 304 - "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:30 +0000] "GET /rest/products/24/reviews HTTP/1.1" 200 30 "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:31 +0000] "GET /rest/user/whoami HTTP/1.1" 304 - "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:31 +0000] "GET /rest/products/1/reviews HTTP/1.1" 200 553 "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:31 +0000] "GET /rest/products/1/reviews HTTP/1.1" 200 553 "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
::ffff:172.17.0.1 - - [16/Jun/2025:17:01:31 +0000] "GET /rest/products/1/reviews HTTP/1.1" 304 - "http://localhost:3000/" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0"
```
This shows web server logs.

## Impact

The exposure of access logs through an unprotected `/support/logs` endpoint represents a significant security vulnerability with multiple potential impacts:

**Information Disclosure:**
- Access logs contain detailed information about user behavior, including IP addresses, user agents, and browsing patterns
- Attackers can analyze request patterns to identify potential vulnerabilities or sensitive endpoints
- User privacy is compromised as their browsing activities are exposed to unauthorized parties

**Reconnaissance and Attack Planning:**
- Logs reveal the application's internal structure, API endpoints, and functionality
- Attackers can identify frequently accessed resources and potential targets for further exploitation
- Error patterns in logs may reveal system weaknesses or misconfigurations

**Session and Authentication Analysis:**
- Request logs may contain session tokens, authentication headers, or other sensitive parameters in URLs
- Attackers can potentially hijack active sessions or identify authentication bypass opportunities
- User identification through correlation of IP addresses and request patterns

**Compliance and Legal Issues:**
- Exposure of user data may violate privacy regulations (GDPR, CCPA, etc.)
- Breach of user trust and potential legal liability
- Failure to protect audit logs may violate industry compliance requirements

**Operational Security Risks:**
- Logs may contain internal system information, server configurations, or deployment details
- Potential exposure of administrative activities and system maintenance patterns
- Risk of log tampering or deletion by malicious actors with access

This vulnerability demonstrates a failure in both access control and monitoring, as the logs are publicly accessible without authentication, and there's likely no alerting mechanism to detect unauthorized access to these sensitive files.

## Classification
This vulnerability is classified as **A09:2021 - Security Logging and Monitoring Failures** for several key reasons:

**Primary Classification:**
- **Inadequate Logging Protection**: The application fails to properly secure its log files, making them publicly accessible without authentication
- **Missing Access Controls**: No authorization mechanisms are in place to restrict access to sensitive audit logs
- **Insufficient Monitoring**: There's no detection or alerting for unauthorized access to log files

**Secondary Classifications:**
This vulnerability also exhibits characteristics of:
- **A01:2021 - Broken Access Control**: The `/support/logs` endpoint lacks proper access restrictions
- **A05:2021 - Security Misconfiguration**: Improper configuration of the web server or application allowing direct access to log files

**Why This is Primarily A09:**
While access control failures are involved, the core issue is the improper handling and protection of security-relevant logs. The OWASP Top 10 specifically identifies "insufficient logging and monitoring" as including scenarios where:
- Auditable events are not logged appropriately
- Logs are stored insecurely or are accessible to unauthorized users
- Log integrity is not maintained
- Monitoring and alerting systems are inadequate

The exposure of detailed access logs containing user behavior, system information, and potential security events directly aligns with the security logging and monitoring failures category, making this the most appropriate primary classification.

## Remediation
To address this security logging and monitoring failure, implement the following remediation strategies:

### Immediate Actions

**1. Restrict Log File Access**
- Remove public access to the `/support/logs` endpoint immediately
- Implement proper authentication and authorization controls for log access
- Move log files outside the web-accessible directory structure
- Configure web server to deny direct access to log files (e.g., `.htaccess` rules for Apache)

**2. Implement Access Controls**
- Require administrative authentication for log file access
- Use role-based access control (RBAC) to limit log access to authorized personnel only
- Implement multi-factor authentication for administrative accounts
- Create audit trails for who accesses log files and when

### Long-term Security Improvements

**3. Secure Log Management**
- Implement centralized logging with proper access controls
- Use log aggregation tools (ELK Stack, Splunk, etc.) with authentication
- Encrypt log files both in transit and at rest
- Implement log rotation and secure archival procedures
- Set appropriate file permissions (e.g., 640 or 600) on log files

**4. Enhanced Monitoring and Alerting**
- Implement real-time monitoring for unauthorized access attempts to log files
- Set up alerts for suspicious patterns in log access
- Monitor for log file modifications or deletions
- Create dashboards for security event visualization
- Implement automated incident response for critical security events

**5. Log Content Security**
- Review and sanitize log content to remove sensitive information
- Avoid logging passwords, session tokens, or personal data
- Implement log masking for sensitive fields (credit card numbers, SSNs)
- Use structured logging formats for better parsing and analysis
- Implement log integrity checks to detect tampering

**6. Compliance and Governance**
- Establish log retention policies based on regulatory requirements
- Document log access procedures and approval processes
- Regular security audits of logging infrastructure
- Train staff on proper log handling and security procedures
- Implement data classification for different types of log data
