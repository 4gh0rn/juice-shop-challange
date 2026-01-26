# OWASP Juice Shop - Security Challenge Documentation

## Table of Contents

- [About This Repository](#about-this-repository)
- [Challenges Overview](#challenges-overview)
- [Challenge Documentation](#challenge-documentation)
- [Tools & Technologies](#tools--technologies)
- [Learning Outcomes](#learning-outcomes)
- [Security Disclaimer](#security-disclaimer)
- [References](#references)

---

## About This Repository

This repository contains comprehensive documentation of security challenges completed as part of my DevSecOps training program using the [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) - an intentionally insecure web application designed for security training.

### Purpose
The primary objectives of this project are:
- **Educational Learning:** Understanding common web application vulnerabilities
- **Practical Application:** Hands-on experience with ethical hacking techniques
- **Security Awareness:** Recognizing and mitigating security risks in web applications
- **Documentation Skills:** Creating and understanding clear reproducible security documentations

### Repository Contents
This repository includes:
- Detailed documentation for each completed challenge
- Step-by-step reproduction guides
- Mitigation strategies and best practices

**IMPORTANT:** All content in this repository is created **exclusively for educational purposes**. All activities were performed in a controlled, legal environment using the OWASP Juice Shop intentionally vulnerable application.

---

## Challenges Overview

| # | Challenge Name | Category | Difficulty | Status |
|---|----------------|----------|------------|-------|
| 1 | [Login as Jim](challenges/login_as_jim.md) | Injection | ⭐⭐⭐ | ✅ Completed |
| 2 | [Route Hunter - SPA Route Discovery](challenges/route_hunter_headless_gobuster.md) | Reconnaissance / Route Enumeration / SPA Testing | ⭐⭐⭐⭐ | ✅ Completed |
| 3 | [Restore Progress - Backup Challenge](challenges/restore_progress_backup.md) | API Manipulation / IDOR | ⭐⭐⭐ | ✅ Completed |
| 4 | [Empty User Registration](challenges/empty_user_registration.md) | Input Validation / Broken Authentication | ⭐⭐ | ✅ Completed |
| 5 | [Password Strength](challenges/password_strength_brute_force.md) | Broken Authentication / Brute Force | ⭐⭐ | ✅ Completed |
| 6 | [Deprecated Interface](challenges/deprecated_interface.md) | Insecure File Upload / Client-Side Validation Bypass | ⭐⭐ | ✅ Completed |
| 7 | [Meta Geo Stalking](challenges/meta_geo_stalking.md) | Information Disclosure / Metadata Exposure | ⭐⭐⭐ | ✅ Completed |
| 8 | [API-only XSS](challenges/api_only_xss.md) | Cross-Site Scripting (XSS) / Persistent XSS | ⭐⭐⭐ | ✅ Completed |
| 9 | [Admin Registration](challenges/admin_registration.md) | Privilege Escalation / Broken Access Control | ⭐⭐⭐ | ✅ Completed |

### Category Distribution
- **[Injection]:** 1 challenge(s)
- **[Reconnaissance / Route Enumeration / SPA Testing]:** 1 challenge(s)
- **[API Manipulation / IDOR]:** 1 challenge(s)
- **[Input Validation / Broken Authentication]:** 1 challenge(s)
- **[Broken Authentication / Brute Force]:** 1 challenge(s)
- **[Insecure File Upload / Client-Side Validation Bypass]:** 1 challenge(s)
- **[Information Disclosure / Metadata Exposure]:** 1 challenge(s)
- **[Cross-Site Scripting (XSS) / Persistent XSS]:** 1 challenge(s)
- **[Privilege Escalation / Broken Access Control]:** 1 challenge(s)

---

## Challenge Documentation

Each challenge has been thoroughly documented with the following structure:
- Vulnerability explanation
- Security risks and impact
- Step-by-step exploitation guide
- Mitigation strategies

For detailed information about each challenge, please refer to the individual challenge documentation files linked in the [Challenges Overview](#challenges-overview) table above.

---

## Tools & Technologies

### Primary Tools Used
| Tool | Purpose | Link |
|------|---------|------|
| OWASP Juice Shop | Target application | [Official Site](https://owasp.org/www-project-juice-shop/) |
| Burp Suite | Traffic interception & analysis | [Download](https://portswigger.net/burp) |
| Browser DevTools | Client-side inspection | Built-in |
| Postman/cURL | API testing | [Postman](https://www.postman.com/) |
| Python 3 + requests | HTTP automation and brute-force scripting | [Python](https://www.python.org/) |
| ExifTool | Extract and manipulate image metadata | [ExifTool](https://exiftool.org/) |

### Development Environment
- **Browser:** Chrome or Firefox
- **Operating System:** macOS, Kali Linux or other similar distro
- **Additional Software:** Python 3.11, Node.js, Git, VS Code or Cursor, curl, Burp Suite Community Edition, ExifTool

---

## Learning Outcomes

Through completing these challenges, the following competencies were developed:

### Technical Skills
- ✅ Understanding of common web vulnerabilities (OWASP Top 10)
- ✅ Practical exploitation techniques
- ✅ Security testing methodologies
- ✅ Traffic analysis and manipulation
- ✅ Brute-force attack automation
- ✅ Password security assessment

### Security Awareness
- ✅ Recognition of insecure coding practices
- ✅ Impact assessment of security vulnerabilities
- ✅ Defensive programming strategies
- ✅ Risk mitigation techniques

### Professional Skills
- ✅ Technical documentation
- ✅ Security reporting
- ✅ Video presentation of technical concepts
- ✅ Ethical hacking principles

---

## Security Disclaimer

### Educational Purpose Only
**This repository and all its contents are created exclusively for educational purposes** as part of a DevSecOps training program. All security testing was performed in a controlled environment using the OWASP Juice Shop - an intentionally vulnerable application designed for security training.

### Ethical Guidelines
- ✅ All activities were performed on authorized test systems only
- ✅ No real user data was accessed or compromised
- ✅ No actual systems were harmed
- ✅ Knowledge is intended to improve security, not compromise it

### Legal Notice
**Unauthorized access to computer systems is illegal.** The techniques demonstrated here should only be used:
- In authorized penetration testing engagements
- On systems you own or have explicit written permission to test
- In designated security training environments
- For legitimate security research with proper authorization

**Never use these techniques on production systems or systems you don't own without explicit written authorization.**

---

## References

### OWASP Resources
- [OWASP Top 10 2021](https://owasp.org/www-project-top-ten/)
- [OWASP Juice Shop Project](https://owasp.org/www-project-juice-shop/)
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)

### Additional Learning Resources
- [Web Security Academy by PortSwigger](https://portswigger.net/web-security)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [HackerOne Hacker101](https://www.hacker101.com/)

### Documentation Standards
- [Markdown Guide](https://www.markdownguide.org/)
- [GitHub Documentation Best Practices](https://docs.github.com/en/communities)

---

## Author

**[Uwe Wohlleber]**  
DevSecOps Training - [2025]

---

## License

This documentation is provided as-is for educational purposes. Please refer to the [OWASP Juice Shop License](https://github.com/juice-shop/juice-shop/blob/master/LICENSE) for the application itself.

---

**Last Updated:** [2025-11-16]  
**Repository Version:** 1.0
