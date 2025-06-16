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

## Exploit

Opening up the `package.json` shows the packages and versions for the app.


```json

{
  "name": "juice-shop",
  "version": "6.2.0-SNAPSHOT",
  "description": "An intentionally insecure JavaScript Web Application",
  "homepage": "http://owasp-juice.shop",
  "author": "Björn Kimminich <bjoern.kimminich@owasp.org> (https://kimminich.de)",
  "contributors": [
    "Björn Kimminich",
    "Jannik Hollenbach",
    "Aashish683",
    "greenkeeper[bot]",
    "MarcRler",
    "agrawalarpit14",
    "Scar26",
    "CaptainFreak",
    "Supratik Das",
    "JuiceShopBot",
    "the-pro",
    "Ziyang Li",
    "aaryan10",
    "m4l1c3",
    "Timo Pagel",
    "..."
  ],
  "private": true,
  "keywords": [
    "web security",
    "web application security",
    "webappsec",
    "owasp",
    "pentest",
    "pentesting",
    "security",
    "vulnerable",
    "vulnerability",
    "broken",
    "bodgeit"
  ],
  "dependencies": {
    "body-parser": "~1.18",
    "colors": "~1.1",
    "config": "~1.28",
    "cookie-parser": "~1.4",
    "cors": "~2.8",
    "dottie": "~2.0",
    "epilogue-js": "~0.7",
    "errorhandler": "~1.5",
    "express": "~4.16",
    "express-jwt": "0.1.3",
    "fs-extra": "~4.0",
    "glob": "~5.0",
    "grunt": "~1.0",
    "grunt-angular-templates": "~1.1",
    "grunt-contrib-clean": "~1.1",
    "grunt-contrib-compress": "~1.4",
    "grunt-contrib-concat": "~1.0",
    "grunt-contrib-uglify": "~3.2",
    "hashids": "~1.1",
    "helmet": "~3.9",
    "html-entities": "~1.2",
    "jasmine": "^2.8.0",
    "js-yaml": "3.10",
    "jsonwebtoken": "~8",
    "jssha": "~2.3",
    "libxmljs": "~0.18",
    "marsdb": "~0.6",
    "morgan": "~1.9",
    "multer": "~1.3",
    "pdfkit": "~0.8",
    "replace": "~0.3",
    "request": "~2",
    "sanitize-html": "1.4.2",
    "sequelize": "~4",
    "serve-favicon": "~2.4",
    "serve-index": "~1.9",
    "socket.io": "~2.0",
    "sqlite3": "~3.1.13",
    "z85": "~0.0"
  },
  "devDependencies": {
    "chai": "~4",
    "codeclimate-test-reporter": "~0.5",
    "cross-spawn": "~5.1",
    "eslint": "~4.7",
    "eslint-scope": "3.7.2",
    "form-data": "~2.3",
    "frisby": "~2.0",
    "grunt-cli": "~1.2",
    "http-server": "~0.10",
    "jasmine-reporters": "~2.2",
    "jest": "~22",
    "karma": "~1.7",
    "karma-chrome-launcher": "~2.2",
    "karma-cli": "~1.0",
    "karma-coverage": "~1.1",
    "karma-jasmine": "~1.1",
    "karma-junit-reporter": "~1.2",
    "karma-phantomjs-launcher": "~1.0",
    "karma-safari-launcher": "~1.0",
    "lcov-result-merger": "~1.2",
    "mocha": "~4",
    "nyc": "~11",
    "phantomjs-prebuilt": "~2",
    "protractor": "~5",
    "shelljs": "~0.7",
    "sinon": "~4",
    "sinon-chai": "~2.14",
    "socket.io-client": "~2.0",
    "standard": "~10",
    "stryker": "~0",
    "stryker-api": "~0",
    "stryker-html-reporter": "~0",
    "stryker-jasmine": "~0",
    "stryker-karma-runner": "~0",
    "stryker-mocha-runner": "~0"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/bkimminich/juice-shop.git"
  },
  "bugs": {
    "url": "https://github.com/bkimminich/juice-shop/issues"
  },
  "license": "MIT",
  "scripts": {
    "postinstall": "npm --prefix ./app install ./app && grunt minify",
    "start": "node app",
    "test": "standard && karma start karma.conf.js && nyc --report-dir=./build/reports/coverage/server-tests mocha test/server",
    "frisby": "nyc --report-dir=./build/reports/coverage/api-tests node ./test/apiTests.js",
    "preupdate-webdriver": "npm install",
    "update-webdriver": "webdriver-manager update",
    "preprotractor": "npm run update-webdriver",
    "protractor": "node test/e2eTests.js",
    "stryker": "stryker run stryker.client-conf.js",
    "vagrant": "cd vagrant && vagrant up"
  },
  "engines": {
    "node": ">=6 <=9"
  },
  "standard": {
    "ignore": [
      "/app/private/**",
      "/vagrant/**"
    ],
    "env": {
      "jasmine": true,
      "node": true,
      "browser": true,
      "mocha": true,
      "protractor": true
    },
    "globals": [
      "angular",
      "inject"
    ]
  },
  "nyc": {
    "include": [
      "lib/*.js",
      "routes/*.js"
    ],
    "all": true,
    "reporter": [
      "lcov",
      "text-summary"
    ]
  },
  "jest": {
    "testMatch": [
      "**/test/api/*Spec.js"
    ],
    "testPathIgnorePatterns": [
      "/node_modules/",
      "/app/node_modules/"
    ]
  }
}

```

## Impact
The impact of vulnerable and outdated components is severe and multifaceted, representing one of the most critical security risks in modern applications:

**Direct Exploitation of Known Vulnerabilities**: The exposed `package.json` file reveals specific versions of dependencies that can be cross-referenced against known vulnerability databases (CVE, npm audit, Snyk). Attackers can identify exact exploits for the specific versions in use, eliminating guesswork and enabling targeted attacks against well-documented security flaws.

**Remote Code Execution**: Many component vulnerabilities allow for remote code execution, giving attackers the ability to run arbitrary commands on the server. This can lead to complete system compromise, data theft, malware installation, and use of the compromised system as a launching point for further attacks.

**Data Breach and Information Disclosure**: Vulnerable components often expose sensitive data through various attack vectors including SQL injection, path traversal, or authentication bypasses. The combination of known component versions and their associated vulnerabilities creates a roadmap for accessing confidential information, user data, and business-critical assets.

**Supply Chain Compromise**: Using outdated components creates supply chain risks where attackers can exploit vulnerabilities in third-party libraries to compromise the entire application ecosystem. This is particularly dangerous as these components often run with the same privileges as the main application.

**Privilege Escalation**: Component vulnerabilities frequently allow attackers to escalate privileges within the application or underlying system. This can transform a low-privilege attack into administrative access, enabling attackers to modify system configurations, access restricted areas, and compromise other users' accounts.

**Denial of Service**: Many component vulnerabilities can be exploited to cause application crashes, resource exhaustion, or service disruption. This can result in business downtime, lost revenue, and degraded user experience.

**Lateral Movement**: Compromised applications with vulnerable components can serve as pivot points for attackers to move laterally within the network, accessing other systems, databases, and services that may contain more valuable information or provide additional attack opportunities.

**Compliance and Regulatory Violations**: Using components with known vulnerabilities often violates security compliance requirements such as PCI DSS, HIPAA, SOX, and GDPR. This can result in significant fines, legal liability, and mandatory breach notifications.

**Business Continuity Impact**: Successful exploitation of component vulnerabilities can disrupt business operations, damage customer trust, and require expensive incident response and recovery efforts. The reputational damage can have long-lasting effects on customer relationships and market position.

**Automated Attack Scalability**: Since component vulnerabilities are well-documented and often have publicly available exploits, attackers can automate the discovery and exploitation process across multiple targets, making these attacks highly scalable and cost-effective for malicious actors.

**Zero-Day Risk**: While using outdated components exposes applications to known vulnerabilities, it also increases the risk of zero-day exploits, as security researchers and attackers often focus on popular, widely-used components.

**Cascading Security Failures**: Component vulnerabilities can create cascading effects where the compromise of one component leads to the failure of multiple security controls, authentication mechanisms, and data protection measures throughout the application.

**CVSS Base Score: High to Critical (7.0-10.0)** - The severity depends on the specific vulnerabilities present in the identified component versions, with many component vulnerabilities rated as high or critical due to their potential for remote code execution and system compromise.

**Attack Vector**: Network-based exploitation of known component vulnerabilities
**Authentication Required**: Often none (many component vulnerabilities can be exploited without authentication)
**User Interaction**: None (attacks can be fully automated)
**Scope**: System-wide compromise affecting confidentiality, integrity, and availability

## Classificaition
This vulnerability is classified as **A06:2021 - Vulnerable and Outdated Components** according to the OWASP Top 10 2021. The classification is based on several key factors:

**Primary Classification Criteria:**
- **Known Vulnerability Exposure**: The application uses components with publicly documented security vulnerabilities that can be exploited by attackers
- **Outdated Component Versions**: Dependencies are running versions that are no longer supported or have known security patches available
- **Insufficient Vulnerability Management**: Lack of regular scanning, monitoring, and updating of third-party components and dependencies

**Secondary Risk Factors:**
- **Information Disclosure**: The exposed `package.json` file provides attackers with a complete inventory of components and their exact versions
- **Attack Vector Amplification**: Knowledge of specific component versions enables targeted exploitation of known vulnerabilities
- **Supply Chain Risk**: Reliance on third-party components introduces external security dependencies beyond the organization's direct control

**Risk Rating: HIGH**
- **Likelihood**: High - Automated tools can easily scan for and exploit known component vulnerabilities
- **Impact**: High to Critical - Can lead to remote code execution, data breaches, and complete system compromise
- **Exploitability**: High - Many component vulnerabilities have publicly available exploits and proof-of-concept code

**CWE Classifications:**
- **CWE-1104**: Use of Unmaintained Third Party Components
- **CWE-937**: Using Components with Known Vulnerabilities
- **CWE-1395**: Dependency on Vulnerable Third-Party Component

**Compliance Impact:**
This vulnerability type is specifically addressed in various security frameworks and standards:
- **NIST Cybersecurity Framework**: Supply Chain Risk Management (ID.SC)
- **ISO 27001**: A.14.2.1 Secure development policy
- **PCI DSS**: Requirement 6.2 - Ensure all system components are protected from known vulnerabilities
- **OWASP ASVS**: V14 Configuration Verification Requirements

## Reccomendations
To effectively mitigate vulnerable and outdated components risks, organizations should implement a comprehensive component security management strategy that addresses prevention, detection, and response:

### Inventory and Asset Management

**1. Component Inventory Management**
- Maintain a comprehensive inventory of all third-party components, libraries, frameworks, and dependencies
- Document component versions, sources, licenses, and security status
- Implement automated dependency scanning tools to continuously track component usage
- Use Software Bill of Materials (SBOM) to maintain transparency in component dependencies

**2. Dependency Mapping**
- Create detailed dependency trees showing relationships between components
- Identify transitive dependencies that may introduce vulnerabilities
- Map components to business-critical functions to prioritize security efforts
- Document component ownership and maintenance responsibilities

### Vulnerability Management

**3. Continuous Vulnerability Scanning**
- Implement automated vulnerability scanning tools (e.g., npm audit, Snyk, OWASP Dependency-Check)
- Integrate vulnerability scanning into CI/CD pipelines
- Configure real-time alerts for newly discovered vulnerabilities in used components
- Establish regular scanning schedules for all environments

**4. Vulnerability Assessment and Prioritization**
- Develop risk-based prioritization criteria for component vulnerabilities
- Consider CVSS scores, exploitability, and business impact when prioritizing patches
- Assess whether vulnerable components are actually used in exploitable ways
- Maintain vulnerability databases and threat intelligence feeds

### Update and Patch Management

**5. Proactive Update Strategy**
- Establish regular update cycles for all components and dependencies
- Test updates in staging environments before production deployment
- Implement automated dependency updates where appropriate and safe
- Maintain rollback procedures for problematic updates

**6. Security Patch Management**
- Prioritize security patches based on vulnerability severity and exploitability
- Establish emergency patch procedures for critical vulnerabilities
- Test compatibility of security patches with existing functionality
- Document all patch deployments and maintain change logs

### Secure Development Practices

**7. Component Selection and Evaluation**
- Establish criteria for evaluating third-party components before adoption
- Prefer components from reputable sources with active maintenance
- Evaluate component security history and vulnerability disclosure practices
- Consider alternatives when components become unmaintained or insecure

**8. Secure Configuration Management**
- Remove unused dependencies, features, and components from production systems
- Disable unnecessary functionality in third-party components
- Configure components according to security best practices
- Regularly audit component configurations for security misconfigurations

### Supply Chain Security

**9. Trusted Sources and Repositories**
- Use only trusted package repositories and registries
- Implement package signing verification where available
- Consider using private repositories for internal components
- Monitor for typosquatting and malicious packages in public repositories

**10. Vendor Security Assessment**
- Evaluate security practices of component vendors and maintainers
- Require security commitments and SLAs from critical component providers
- Monitor vendor security advisories and communication channels
- Establish procedures for vendor security incident response

### Development Pipeline Integration

**11. CI/CD Security Integration**
- Integrate security scanning into build and deployment pipelines
- Implement automated security gates that prevent deployment of vulnerable components
- Configure pipeline failures for high-severity vulnerabilities
- Maintain security scanning results and historical trends

**12. Code Review and Testing**
- Include component security review in code review processes
- Test applications with updated components before production deployment
- Implement security regression testing for component updates
- Validate that security patches effectively address identified vulnerabilities

### Monitoring and Detection

**13. Runtime Security Monitoring**
- Implement runtime application security monitoring (RASP) where applicable
- Monitor for exploitation attempts targeting known component vulnerabilities
- Configure alerts for suspicious activities related to vulnerable components
- Maintain security event correlation and analysis capabilities

**14. Threat Intelligence Integration**
- Subscribe to security advisories for all used components
- Integrate threat intelligence feeds into vulnerability management processes
- Monitor security research and proof-of-concept publications
- Participate in security communities and information sharing initiatives

### Incident Response and Recovery

**15. Component Security Incident Response**
- Develop specific incident response procedures for component vulnerabilities
- Establish communication channels with component vendors and security teams
- Implement rapid response capabilities for zero-day component vulnerabilities
- Maintain forensic capabilities for component-related security incidents

**16. Business Continuity Planning**
- Develop contingency plans for critical component vulnerabilities
- Identify alternative components for business-critical dependencies
- Implement backup and recovery procedures for component-related incidents
- Test incident response procedures regularly

### Governance and Compliance

**17. Security Policies and Standards**
- Establish organizational policies for third-party component usage
- Define security requirements for component selection and maintenance
- Implement approval processes for new component adoption
- Create security training programs for development teams

**18. Compliance and Audit**
- Ensure component security practices meet regulatory requirements
- Implement audit trails for component security decisions
- Conduct regular security assessments of component management practices
- Maintain documentation for compliance reporting and audits

### Advanced Security Measures

**19. Container and Infrastructure Security**
- Scan container images for vulnerable components and base images
- Implement container security policies and runtime protection
- Use minimal base images and remove unnecessary components
- Regularly update container images and orchestration platforms

**20. Zero Trust Architecture**
- Implement least privilege access for component interactions
- Use network segmentation to limit component vulnerability impact
- Implement micro-segmentation for containerized applications
- Monitor and control component network communications

This comprehensive approach to component security management helps organizations maintain visibility into their software supply chain, rapidly respond to emerging threats, and minimize the risk of exploitation through vulnerable or outdated components.
