# A05:2021 Security Misconfiguration

## Overview
[Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/) occurs when security settings are not defined, implemented, and maintained properly. This vulnerability can happen at any level of an application stack, including network services, platforms, web servers, application servers, databases, frameworks, custom code, and pre-installed virtual machines, containers, or storage.

Security misconfigurations are commonly a result of insecure default configurations, incomplete or ad hoc configurations, open cloud storage, misconfigured HTTP headers, and verbose error messages containing sensitive information. Not only must all operating systems, frameworks, libraries, and applications be securely configured, but they must be patched and upgraded in a timely fashion.

**Common examples include:**
- Missing appropriate security hardening across any part of the application stack
- Improperly configured permissions on cloud services
- Unnecessary features enabled or installed (e.g., unnecessary ports, services, pages, accounts, or privileges)
- Default accounts and their passwords still enabled and unchanged
- Error handling reveals stack traces or other overly informative error messages to users
- Security settings in application servers, frameworks, libraries, databases, etc., are not set to secure values
- The server does not send security headers or directives, or they are not set to secure values

## Reconasissance
The previous exploit showed there was an `/ftp` endpoint with a list of files. [04 Insecure Design](./04-insecure-design.md) accessed the `aquisitions.md` file. See that walk through to learn how that endpoint was discovered and the results of that research. 

There were other files that could be of interested to an attacker. Clicking on the `package.json.bak` would be interesting since the `package.json` file holds all of the information about the packages used in the application. The application errors showing a stack trace and a version of the web server.
