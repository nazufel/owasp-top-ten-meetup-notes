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


### Reconnaissance
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

### Exploit
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

### Impact

#### Classification

### Remediation