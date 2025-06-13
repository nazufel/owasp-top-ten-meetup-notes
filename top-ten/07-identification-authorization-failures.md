# A7: 2021 - Idenfitication and Authorization Errors

## Overview
[Identification and Authorization Failures](https://owasp.org/Top10/A07_2021-Identification_and_Authorization_Failures/) occur when applications fail to properly confirm user identity, authenticate users, or authorize access to resources. This vulnerability category encompasses weaknesses in session management, authentication mechanisms, and access controls that can allow attackers to compromise passwords, keys, or session tokens, or exploit other implementation flaws to assume other users' identities temporarily or permanently.

Identification and authorization failures can lead to unauthorized access to sensitive data, privilege escalation, account takeover, and complete system compromise. These failures often result from weak authentication mechanisms, improper session management, predictable session identifiers, or missing authorization checks.

**Common examples include:**
- Permits automated attacks such as credential stuffing, where the attacker has a list of valid usernames and passwords
- Permits brute force or other automated attacks
- Permits default, weak, or well-known passwords, such as "Password1" or "admin/admin"
- Uses weak or ineffective credential recovery and forgot-password processes
- Uses plain text, encrypted, or weakly hashed passwords
- Has missing or ineffective multi-factor authentication
- Exposes session identifier in the URL
- Reuses session identifier after successful login
- Does not properly invalidate session IDs during logout or a period of inactivity
- Missing access controls or authorization checks for authenticated functionality
