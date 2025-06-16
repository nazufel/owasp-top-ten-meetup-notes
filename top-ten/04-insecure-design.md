# A4:2021 - Insecure Design

# Overview
[Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/) represents a broad category of weaknesses related to design and architectural flaws. It focuses on risks related to design flaws rather than implementation defects. Insecure design occurs when applications are designed without proper security controls or when security controls are inadequately designed to defend against specific attack vectors.

Insecure design is different from insecure implementation - you can have a secure design but still have implementation flaws, or you can have a perfect implementation of an insecure design. The key distinction is that insecure design cannot be fixed by a perfect implementation as the security controls were never created to defend against specific attacks.

**Common examples include:**
- Missing or ineffective control design to prevent automated attacks
- Lack of business logic validation
- Missing authentication for functionality that should require it
- Insufficient logging and monitoring
- Failure to segregate tenants in a multi-tenant architecture
- Missing rate limiting
- Credential recovery workflows that can be bypassed or manipulated
- Unprotected APIs or endpoints that expose sensitive functionality


## Reconnaissance
The following vulnerability was discovered using nikto trying to figureprint the server. I noticed in the top of the output that `robots.txt` disallowed crawlers at the `/ftp` route. So, of course, I check that out.

```shell
nikto -h http://localhost:3000
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          127.0.0.1
+ Target Hostname:    localhost
+ Target Port:        3000
+ Start Time:         2025-06-07 07:13:29 (GMT-4)
---------------------------------------------------------------------------
+ Server: No banner retrieved
+ /: Retrieved access-control-allow-origin header: *.
+ /: Uncommon header 'x-recruiting' found, with contents: /#/jobs.
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ /robots.txt: Entry '/ftp/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: contains 1 entry which should be manually viewed. See: https://developer.mozilla.org/en-US/docs/Glossary/Robots.txt

```
Running `curl localhost:3000/ftp` returns this `<body>` of text.

```html
<body class="directory">
    <input id="search" type="text" placeholder="Search" autocomplete="off" />
    <div id="wrapper">
      <h1><a href=".">~</a> / <a href="ftp">ftp</a></h1>
      <ul id="files" class="view-tiles"><li><a href="ftp/quarantine" class="icon icon-directory" title="quarantine"><span class="name">quarantine</span><span class="size"></span><span class="date">4/22/2025 10:12:15 PM</span></a></li>
<li><a href="ftp/acquisitions.md" class="icon icon icon-md icon-text" title="acquisitions.md"><span class="name">acquisitions.md</span><span class="size">909</span><span class="date">4/22/2025 10:12:15 PM</span></a></li>
<li><a href="ftp/announcement_encrypted.md" class="icon icon icon-md icon-text" title="announcement_encrypted.md"><span class="name">announcement_encrypted.md</span><span class="size">369237</span><span class="date">4/22/2025 10:12:15 PM</span></a></li>
<li><a href="ftp/coupons_2013.md.bak" class="icon icon icon-bak icon-default" title="coupons_2013.md.bak"><span class="name">coupons_2013.md.bak</span><span class="size">131</span><span class="date">4/22/2025 10:12:15 PM</span></a></li>
<li><a href="ftp/eastere.gg" class="icon icon icon-gg icon-default" title="eastere.gg"><span class="name">eastere.gg</span><span class="size">324</span><span class="date">4/22/2025 10:12:15 PM</span></a></li>
<li><a href="ftp/encrypt.pyc" class="icon icon icon-pyc icon-default" title="encrypt.pyc"><span class="name">encrypt.pyc</span><span class="size">573</span><span class="date">4/22/2025 10:12:15 PM</span></a></li>
<li><a href="ftp/incident-support.kdbx" class="icon icon icon-kdbx icon-default" title="incident-support.kdbx"><span class="name">incident-support.kdbx</span><span class="size">3246</span><span class="date">4/22/2025 10:12:15 PM</span></a></li>
<li><a href="ftp/legal.md" class="icon icon icon-md icon-text" title="legal.md"><span class="name">legal.md</span><span class="size">3047</span><span class="date">6/7/2025 11:03:43 AM</span></a></li>
<li><a href="ftp/package.json.bak" class="icon icon icon-bak icon-default" title="package.json.bak"><span class="name">package.json.bak</span><span class="size">4291</span><span class="date">4/22/2025 10:12:15 PM</span></a></li>
<li><a href="ftp/suspicious_errors.yml" class="icon icon icon-yml icon-text" title="suspicious_errors.yml"><span class="name">suspicious_errors.yml</span><span class="size">723</span><span class="date">4/22/2025 10:12:15 PM</span></a></li></ul>
    </div>
  </body>

```

## Exploit
It's a list of sensative files on a file server. No authentication needed. `acquisitions.md` looks interesting. Curling for that, `curl localhost:3000/ftp/aqcuisitions.md` returns:

```md
# Planned Acquisitions

> This document is confidential! Do not distribute!

Our company plans to acquire several competitors within the next year.
This will have a significant stock market impact as we will elaborate in
detail in the following paragraph:

Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy
eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam
voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet
clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit
amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam
nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat,
sed diam voluptua. At vero eos et accusam et justo duo dolores et ea
rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem
ipsum dolor sit amet.

Our shareholders will be excited. It's true. No fake news.

``` 
Found what looks to be a real and condiential document. The business impact of this information getting out would be substantial.

## Impact
The exposure of confidential business documents through an unprotected FTP directory represents a critical security vulnerability with severe business and legal implications:

**Business Impact:**
- **Market Manipulation Risk**: Exposure of planned acquisitions could lead to insider trading, stock price manipulation, and regulatory violations
- **Competitive Disadvantage**: Competitors gain access to strategic business plans, potentially undermining acquisition strategies and negotiations
- **Financial Losses**: Premature disclosure may increase acquisition costs, reduce negotiating power, or cause deals to fall through entirely
- **Shareholder Impact**: Stock price volatility and potential lawsuits from shareholders affected by information leakage

**Legal and Regulatory Consequences:**
- **Securities Law Violations**: Unauthorized disclosure of material non-public information may violate SEC regulations and securities laws
- **Breach of Fiduciary Duty**: Company officers may face legal liability for failing to protect confidential business information
- **Contractual Violations**: Breach of non-disclosure agreements with acquisition targets or business partners
- **Regulatory Investigations**: Potential SEC, FTC, or other regulatory body investigations into information handling practices

**Operational Security Risks:**
- **Data Breach Exposure**: The FTP directory contains multiple sensitive files including encrypted documents, backup files, and system configurations
- **System Compromise**: Files like `encrypt.pyc`, `package.json.bak`, and `suspicious_errors.yml` may reveal system vulnerabilities or configuration details
- **Credential Exposure**: The `incident-support.kdbx` file appears to be a KeePass database that could contain administrative credentials
- **Further Attack Vectors**: Exposed backup files and configuration data provide attackers with detailed system information for advanced persistent threats

**Reputational Damage:**
- **Loss of Trust**: Customers, partners, and investors lose confidence in the company's ability to protect sensitive information
- **Brand Damage**: Public exposure of the security failure damages corporate reputation and market position
- **Industry Standing**: Loss of credibility within the business community and potential exclusion from future partnerships

**Privacy and Compliance Issues:**
- **Data Protection Violations**: Potential violations of data protection regulations depending on what personal information is contained in exposed files
- **Industry Compliance**: Failure to meet industry-specific security standards and compliance requirements
- **Audit Failures**: Internal and external audits will likely identify this as a critical control failure

This vulnerability demonstrates a fundamental failure in access control design, where sensitive business documents are stored in a publicly accessible location without any authentication or authorization mechanisms.

## Classification
This vulnerability is classified as **A04:2021 - Insecure Design** for several key reasons:

**Primary Classification:**
- **Fundamental Design Flaw**: The application was designed with an inherent security weakness - storing sensitive business documents in a publicly accessible FTP directory without any access controls
- **Missing Security Controls by Design**: The system lacks basic authentication, authorization, or encryption mechanisms for protecting confidential data
- **Architectural Security Failure**: The design fails to consider the security implications of exposing sensitive business information through an unprotected interface

**Why This is Insecure Design vs. Other Categories:**
While this vulnerability could potentially be classified under other OWASP categories, it primarily represents an insecure design because:

- **Not Implementation Error**: This isn't a coding mistake or misconfiguration - it's a fundamental design decision to make sensitive files publicly accessible
- **Not Access Control Failure**: The system is working as designed; there simply are no access controls designed into the system
- **Not Security Misconfiguration**: The FTP service appears to be configured correctly - the problem is the intentional design choice to expose sensitive data

**Design Principles Violated:**
- **Principle of Least Privilege**: The system grants unrestricted access to sensitive documents without any authentication
- **Defense in Depth**: No layered security controls protect the confidential business information
- **Secure by Default**: The default behavior exposes sensitive data rather than protecting it
- **Fail Securely**: The system fails open, providing access to sensitive information when security controls are absent

**Secondary Classifications:**
This vulnerability also exhibits characteristics of:
- **A01:2021 - Broken Access Control**: Complete absence of access restrictions on sensitive documents
- **A05:2021 - Security Misconfiguration**: Potential web server misconfiguration allowing directory traversal and file access

However, the core issue remains the fundamental design decision to store and expose sensitive business documents without implementing appropriate security controls from the ground up, making this primarily an insecure design vulnerability.

## Remediation
To address this insecure design vulnerability, implement the following comprehensive remediation strategies:

### Immediate Actions

**1. Remove Public Access**
- Immediately disable or restrict access to the publicly accessible FTP directory
- Move all sensitive documents to a secure, non-web-accessible location
- Implement emergency access controls to prevent further unauthorized access
- Conduct immediate damage assessment to determine what information was accessed

**2. Implement Authentication and Authorization**
- Design and implement proper user authentication mechanisms for document access
- Create role-based access control (RBAC) system with appropriate permission levels
- Require multi-factor authentication for access to sensitive business documents
- Implement session management and timeout controls for authenticated users

### Secure Design Implementation

**3. Redesign Document Storage Architecture**
- Move sensitive documents to a secure document management system with proper access controls
- Implement encrypted storage for all confidential business information
- Design secure APIs for document access with proper authentication and authorization
- Use secure protocols (HTTPS, SFTP) instead of unencrypted FTP for any file transfers

**4. Access Control Framework**
- Implement principle of least privilege - users only get access to documents they specifically need
- Create approval workflows for accessing highly sensitive documents (acquisition plans, financial data)
- Implement document classification system (Public, Internal, Confidential, Restricted)
- Design audit logging for all document access attempts and activities

**5. Data Protection Controls**
- Encrypt sensitive documents both at rest and in transit
- Implement data loss prevention (DLP) controls to prevent unauthorized data exfiltration
- Use digital rights management (DRM) for highly sensitive documents
- Implement watermarking and tracking for confidential documents

### Long-term Security Architecture

**6. Secure Development Practices**
- Integrate security requirements into the design phase of all systems
- Conduct threat modeling for document storage and access systems
- Implement security design reviews before system deployment
- Establish secure coding standards and practices for document handling

**7. Monitoring and Detection**
- Implement real-time monitoring for unauthorized access attempts to sensitive documents
- Set up alerts for unusual document access patterns or bulk downloads
- Create security dashboards for document access monitoring
- Implement automated incident response for security violations

**8. Compliance and Governance**
- Establish document classification and handling policies
- Implement data retention and secure disposal procedures
- Create incident response procedures for data exposure events
- Conduct regular security assessments of document storage systems

### Business Process Improvements

**9. Information Governance**
- Establish clear policies for storing and sharing sensitive business information
- Implement document lifecycle management with appropriate security controls
- Create secure collaboration platforms for sharing sensitive documents with external parties
- Establish vendor management procedures for secure information sharing

**10. Training and Awareness**
- Train employees on proper handling of sensitive business documents
- Establish clear guidelines for document classification and protection
- Implement security awareness programs focused on information protection
- Create incident reporting procedures for potential data exposure

### Technical Controls

**11. Network Security**
- Implement network segmentation to isolate document storage systems
- Use VPN or other secure access methods for remote document access
- Implement intrusion detection and prevention systems
- Configure firewalls to restrict access to document storage systems

**12. Backup and Recovery**
- Implement secure backup procedures for sensitive documents
- Encrypt all backup media and storage
- Test backup and recovery procedures regularly
- Implement secure disposal procedures for backup media

This comprehensive remediation approach addresses the fundamental design flaws while implementing defense-in-depth security controls to protect sensitive business information from unauthorized access and disclosure.
