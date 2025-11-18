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
- Video demonstrations of exploitation techniques via Loom videos
- Step-by-step reproduction guides
- Mitigation strategies and best practices

**IMPORTANT:** All content in this repository is created **exclusively for educational purposes**. All activities were performed in a controlled, legal environment using the OWASP Juice Shop intentionally vulnerable application.

---

## Challenges Overview

| # | Challenge Name | Category | Difficulty | Video | Status |
|---|----------------|----------|------------|-------|--------|
| 1 | [Login as Jim](challenges/login_as_jim.md) | Injection | ⭐⭐⭐ | [🎥 Watch](https://www.loom.com/share/94b1cb88cb4641c38633bd457486a31b) | ✅ Completed |
| 2 | [Route Hunter - SPA Route Discovery](challenges/route_hunter_headless_gobuster.md) | Reconnaissance / Route Enumeration / SPA Testing | ⭐⭐⭐⭐ | [🎥 Watch](https://www.loom.com/share/d6ce01e04ee44f248b0cfc3df5909540) | ✅ Completed |
| 3 | [Restore Progress - Backup Challenge](challenges/restore_progress_backup.md) | API Manipulation / IDOR | ⭐⭐⭐ | [🎥 Watch](link) | ✅ Completed |
| 4 | [Empty User Registration](challenges/empty_user_registration.md) | Input Validation / Broken Authentication | ⭐⭐ | [🎥 Watch](https://www.loom.com/share/8f4b7728e4ad4a039ef589dc9c5521a1) | ✅ Completed |
| 5 | [Password Strength](challenges/password_strength_brute_force.md) | Broken Authentication / Brute Force | ⭐⭐ | [🎥 Watch](https://www.loom.com/share/ef76e0596fdf4bb4b9a08e5406bccc63) | ✅ Completed |
| 6 | [Deprecated Interface](challenges/deprecated_interface.md) | Insecure File Upload / Client-Side Validation Bypass | ⭐⭐ | [🎥 Watch](https://www.loom.com/share/22e248028a6c424a977ded94f15a6b14) | ✅ Completed |
| 7 | [Meta Geo Stalking](challenges/meta_geo_stalking.md) | Information Disclosure / Metadata Exposure | ⭐⭐⭐ | [🎥 Watch](https://www.loom.com/share/b915558227f54eba8819e6928da5583d) | ✅ Completed |
| 8 | [API-only XSS](challenges/api_only_xss.md) | Cross-Site Scripting (XSS) / Persistent XSS | ⭐⭐⭐ | [🎥 Watch](https://www.loom.com/share/5ba996113df349d68403768994bcc221) | ✅ Completed |
| 9 | [Admin Registration](challenges/admin_registration.md) | Privilege Escalation / Broken Access Control | ⭐⭐⭐ | [🎥 Watch](https://www.loom.com/share/8ff082c6c0294bf596a040a2671c4c84) | ✅ Completed |

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
- Video demonstration
- Mitigation strategies

### Challenge 1: [Login as Jim]
**Category:** [Injection] | **Difficulty:** ⭐⭐⭐

**Brief:** Log in with Jim's user account.

**Security Risk:** SQL Injection can allow attackers to bypass authentication, access sensitive user data, and compromise the entire database, leading to severe data breaches and potential legal or financial consequences.

**Documentation:** [📄 Read Full Documentation](./challenges/login_as_jim.md)  
**Video:** [🎥 Watch Demo](https://www.loom.com/share/94b1cb88cb4641c38633bd457486a31b) _(Duration: 2.45 minutes)_

---

### Challenge 2: Route Hunter - SPA Route Discovery
**Category:** Reconnaissance / Route Enumeration / SPA Testing | **Difficulty:** ⭐⭐⭐⭐

**Brief:** Develop and use a custom headless tool to enumerate hidden client-side routes in a Single Page Application (OWASP Juice Shop) using Selenium-based browser automation and differential analysis techniques.

**Security Risk:** Unprotected or discoverable client-side routes can leak endpoints exposing sensitive data or administrative panels, greatly expanding the application's attack surface.

**Documentation:** [📄 Read Full Documentation](./challenges/route_hunter_headless_gobuster.md)  
**Video:** [🎥 Watch Demo](https://www.loom.com/share/d6ce01e04ee44f248b0cfc3df5909540) _(Duration: 3:22 minutes)_

---

### Challenge 3: Restore Progress - Backup Challenge (Bonus)
**Category:** API Manipulation / Insecure Direct Object Reference (IDOR) | **Difficulty:** ⭐⭐⭐

**Brief:** Exploit an insecure backup/restore mechanism by manipulating a PUT request with a continue code to restore challenge progress without requiring the actual backup file.

**Security Risk:** Insecure Direct Object References and poorly designed API endpoints allow unauthorized manipulation of user data and state, potentially bypassing business logic and access controls.

**Documentation:** [📄 Read Full Documentation](./challenges/restore_progress_backup.md)  
**Video:** [🎥 Watch Demo](https://www.loom.com/share/ec71779cd20d4d97a90b16d2ae5868e6) _(Duration: 1:34 minutes)_

---

### Challenge 4: Empty User Registration
**Category:** Input Validation / Broken Authentication | **Difficulty:** ⭐⭐

**Brief:** Exploit missing server-side input validation by intercepting registration and login requests with Burp Suite, emptying all required fields, and successfully registering and authenticating with empty credentials.

**Security Risk:** Missing server-side validation allows attackers to bypass authentication controls, create accounts with invalid data, and gain unauthorized access, leading to data integrity issues and security breaches.

**Documentation:** [📄 Read Full Documentation](./challenges/empty_user_registration.md)  
**Video:** [🎥 Watch Demo](https://www.loom.com/share/8f4b7728e4ad4a039ef589dc9c5521a1) _(Duration: 2.18 minutes)_

---

### Challenge 5: Password Strength
**Category:** Broken Authentication / Brute Force | **Difficulty:** ⭐⭐

**Brief:** Exploit weak password security by using a brute-force attack with a password list to successfully authenticate to a user account with a weak password.

**Security Risk:** Weak passwords and lack of brute-force protection allow attackers to systematically guess passwords and gain unauthorized access to user accounts, leading to account takeover, data breaches, and potential credential stuffing attacks across multiple services.

**Documentation:** [📄 Read Full Documentation](./challenges/password_strength_brute_force.md)  
**Video:** [🎥 Watch Demo](https://www.loom.com/share/ef76e0596fdf4bb4b9a08e5406bccc63) _(Duration: 1.39 minutes)_

---

### Challenge 6: Deprecated Interface
**Category:** Insecure File Upload / Client-Side Validation Bypass | **Difficulty:** ⭐⭐

**Brief:** Bypass client-side file type validation by examining JavaScript code and using the browser's file picker to upload unauthorized file types (e.g., XML) instead of the restricted PDF format.

**Security Risk:** Insecure file upload vulnerabilities allow attackers to upload malicious files, potentially leading to server-side code execution, malware distribution, data exfiltration, and complete system compromise. Relying solely on client-side validation provides a false sense of security.

**Documentation:** [📄 Read Full Documentation](./challenges/deprecated_interface.md)  
**Video:** [🎥 Watch Demo](https://www.loom.com/share/22e248028a6c424a977ded94f15a6b14) _(Duration: 2.5 minutes)_

---

### Challenge 7: Meta Geo Stalking
**Category:** Information Disclosure / Metadata Exposure | **Difficulty:** ⭐⭐⭐

**Brief:** Extract GPS coordinates from images on the photo wall using ExifTool, convert coordinates to location names (e.g., "Daniel Boone National Forest"), and use this location information to discover a user's password and gain unauthorized access to their account.

**Security Risk:** Metadata exposure in images can reveal sensitive location information, enabling physical stalking, password discovery, social engineering attacks, and privacy violations. Users often create passwords based on personal information including favorite locations, making metadata extraction a significant security risk.

**Documentation:** [📄 Read Full Documentation](./challenges/meta_geo_stalking.md)  
**Video:** [🎥 Watch Demo](https://www.loom.com/share/b915558227f54eba8819e6928da5583d) _(Duration: 2.32 minutes)_

---

### Challenge 8: API-only XSS
**Category:** Cross-Site Scripting (XSS) / Persistent XSS | **Difficulty:** ⭐⭐⭐

**Brief:** Inject a persistent XSS payload through the product API endpoint using a PUT request. Update a product's description field with a malicious script that is stored in the database and executes automatically whenever the product is viewed, demonstrating the danger of insufficient input validation in API endpoints.

**Security Risk:** Persistent XSS attacks allow attackers to inject malicious scripts that are stored in the database and executed for all users viewing the compromised content. This can lead to session hijacking, account takeover, data theft, and complete user account compromise. API endpoints without proper input validation and output encoding are prime targets for XSS attacks.

**Documentation:** [📄 Read Full Documentation](./challenges/api_only_xss.md)  
**Video:** [🎥 Watch Demo](https://www.loom.com/share/5ba996113df349d68403768994bcc221) _(Duration: 2.47 minutes)_

---

### Challenge 9: Admin Registration
**Category:** Privilege Escalation / Broken Access Control | **Difficulty:** ⭐⭐⭐

**Brief:** Exploit broken access control in the user registration process by intercepting the registration request with Burp Suite and adding a `role` attribute set to `"admin"`. This allows creating a new user account with administrative privileges, demonstrating how trusting client-side input for authorization decisions enables privilege escalation.

**Security Risk:** Broken access control allows attackers to escalate privileges and gain unauthorized administrative access. This can lead to complete system compromise, data breaches, unauthorized access to all user accounts, and manipulation of business logic. Applications must never trust client-side input for authorization decisions.

**Documentation:** [📄 Read Full Documentation](./challenges/admin_registration.md)  
**Video:** [🎥 Watch Demo](https://www.loom.com/share/8ff082c6c0294bf596a040a2671c4c84) _(Duration: 2.35 minutes)_

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
