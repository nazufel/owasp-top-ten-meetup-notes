# A10:2021 - Server-Side Request Forgery (SSRF)

## Overview
[Server-Side Request Forgery (SSRF)](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/) flaws occur whenever a web application is fetching a remote resource without validating the user-supplied URL. It allows an attacker to coerce the application to send a crafted request to an unexpected destination, even when protected by a firewall, VPN, or another type of network access control list (ACL).

SSRF vulnerabilities can allow attackers to access internal systems, services, and data that are not directly accessible from the internet. These attacks can lead to unauthorized access to internal services, data exfiltration, port scanning of internal networks, and in some cases, remote code execution on internal systems.

**Common examples include:**
- Applications that fetch remote resources without validating user-supplied URLs
- Applications that allow users to import data from URLs without proper validation
- Webhook functionality that doesn't validate callback URLs
- PDF generators that fetch remote resources based on user input
- Image processing services that fetch images from user-provided URLs
- Applications that proxy requests to third-party services without proper validation

## Reconnaissance

## Exploit

### Impact

#### Classification
