# A6:2021 - Vulnerable and Outdated Components

## Overview
[Vulnerable and Outdated Components](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/) are a category of security risks that arise when software is dependent on components, libraries, or frameworks that are outdated, unsupported, or have known vulnerabilities.

The use of such components can expose applications to various attacks, as attackers can exploit known flaws to compromise the application's security. This can lead to data breaches, server takeover, or other significant impacts, especially since these components often run with the same privileges as the application itself.

**Common examples include:**
- Using components with known vulnerabilities (e.g., CVE-2017-5638 in Apache Struts 2)
- Running outdated versions of operating systems, web/application servers, database management systems (DBMS), applications, APIs, and all components, runtime environments, and libraries
- Not scanning for vulnerabilities regularly and not subscribing to security bulletins related to the components you use
- Not fixing or upgrading the underlying platform, frameworks, and dependencies in a risk-based, timely fashion
- Software developers not testing the compatibility of updated, upgraded, or patched libraries
- Not securing the components' configurations (see A05:2021-Security Misconfiguration)
- Using components from untrusted sources or that are not actively maintained
- Failing to remove unused dependencies, unnecessary features, components, files, and documentation

## Reconasissance
The reconasissance for this vulnerability started in the previous entry. If you have not done [05 Security Misconfiguration](./05-security-misconfiguration.md) then go back and do that one first.