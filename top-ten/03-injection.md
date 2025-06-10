# A3-2021 - Injection

## Overview
[Injection](https://owasp.org/Top10/A03_2021-Injection/) vulnerabilities occur when untrusted data is sent to an interpreter as part of a command or query. This happens when user-supplied data is not properly validated, filtered, or sanitized before being processed by the application. Injection flaws are particularly dangerous because they can allow attackers to execute unintended commands or access data without proper authorization.

The most common types of injection attacks include SQL injection, NoSQL injection, OS command injection, and LDAP injection. These vulnerabilities arise when applications construct queries or commands by concatenating user input directly into the query string, rather than using parameterized queries or prepared statements.

Injection attacks can result in data loss, corruption, or disclosure to unauthorized parties, loss of accountability, denial of access, and sometimes complete host takeover. The business impact depends on the needs of the application and data protection requirements, but injection vulnerabilities are consistently found in applications and can be devastating when successfully exploited.
