# [Route Hunter - SPA Route Discovery] - [Difficulty Level ⭐⭐⭐⭐]

## Overview

**Category:** [Reconnaissance / Route Enumeration / SPA Testing]  
**Difficulty:** [4 stars]  
**OWASP Top 10:** A05:2021 – Security Misconfiguration / Information Disclosure

### Brief Description
This challenge involves building a custom headless route discovery tool called "Route Hunter" (CLI: `spa-enum`) that automates the discovery of hidden routes and endpoints in Single Page Applications (SPAs). Unlike traditional directory enumeration tools that rely on HTTP status codes, Route Hunter uses Selenium-based browser automation to render JavaScript-heavy applications and performs differential analysis to detect routes. The tool tests both hash-based routes (`/#route`) and History API routes (`/route`), comparing page characteristics (title, H1, content length) against a baseline to identify valid routes. By developing and using this tool against the Juice Shop application, we demonstrate how attackers can systematically discover client-side routes that may not be detectable through traditional enumeration methods.

---

## Security Vulnerability Explained

### What is SPA Route Enumeration?
SPA (Single Page Application) route enumeration is a specialized reconnaissance technique used to discover client-side routes in JavaScript-heavy web applications. Unlike traditional server-side directory enumeration that relies on HTTP status codes, SPA route enumeration requires rendering the JavaScript application in a browser to detect routes. Modern SPAs use client-side routing (React Router, Vue Router, Angular Router, etc.) where routes are handled by JavaScript rather than the server. These routes may not return different HTTP status codes, making them invisible to traditional enumeration tools. Route Hunter uses differential analysis by comparing page characteristics (title, H1 headings, content length) against a baseline to identify valid routes, even when they don't trigger server-side responses.

### Dangers and Risks
Directory enumeration exposes critical security risks to web applications:

- **Information Disclosure:** Hidden endpoints may expose sensitive data, configuration files, backup files, or API documentation that reveals application structure and potential attack vectors.
- **Unauthorized Access:** Discovered administrative panels or API endpoints may have weak authentication or be misconfigured, allowing unauthorized access to sensitive functionality.
- **Attack Surface Expansion:** Each discovered endpoint increases the potential attack surface, providing more opportunities for exploitation of vulnerabilities.
- **Business Impact:** Exposed endpoints can lead to data breaches, service disruption, or compliance violations, especially if they reveal customer data or internal business logic.
- **Real-world Examples:**
  - *GitHub (2019):* Exposed AWS credentials through discovered `.env` files in public repositories.
  - *Multiple incidents:* Backup files (`.bak`, `.old`) discovered through enumeration have led to source code leaks and credential exposure.

### Why This Matters
SPA route enumeration is particularly important as modern web applications increasingly rely on client-side routing. Traditional enumeration tools fail to detect these routes because they don't trigger different HTTP responses. Understanding how SPAs handle routing and how to enumerate client-side routes is crucial for comprehensive security assessments. Building custom enumeration tools demonstrates deeper understanding of browser automation, JavaScript rendering, differential analysis techniques, and security testing methodologies. This knowledge is essential for testing modern web applications that heavily rely on client-side frameworks.

---

## Challenge Documentation

### Challenge Description
The "Route Hunter" challenge involves developing a custom headless route discovery tool specifically designed for Single Page Applications (SPAs). The tool uses Selenium WebDriver to render JavaScript-heavy applications and performs differential analysis to detect routes. Unlike traditional enumeration tools, Route Hunter compares page characteristics (title, H1 headings, content length) against a baseline to identify valid routes, even when they don't trigger different HTTP status codes. The tool tests both hash-based routes (`/#route`) and History API routes (`/route`), making it effective for modern SPAs using various routing strategies. When applied to the Juice Shop application, this tool helps discover hidden client-side routes, administrative interfaces, and other resources that traditional enumeration tools would miss.

### Prerequisites
- [x] Python 3.7+ installed
- [x] Understanding of Single Page Applications (SPAs) and client-side routing
- [x] Basic knowledge of Selenium WebDriver and browser automation
- [x] OWASP Juice Shop application running locally or remotely
- [x] Wordlist file for route enumeration (e.g., `wordlist.txt` with common route names)
- [x] Chrome or Firefox browser installed (for Selenium)
- [x] Understanding of JavaScript frameworks and client-side routing mechanisms

---

## Exploitation Steps

### Step 1: Tool Development - Route Hunter
**Goal:** Build a custom headless SPA route discovery tool with Selenium-based rendering and differential analysis.

**Actions:**
1. Set up Python project structure with proper package organization (`route_hunter/` package)
2. Implement CLI entry point (`cli.py`) with argument parsing for:
   - Target URL (positional argument)
   - Wordlist file (positional argument)
   - `--headless` flag for headless browser mode
   - `--variants` to test hash-based (`hash`) and/or History API (`history`) routes
   - `--delay` for delay between requests
   - `--timeout` for page load timeout
   - `--browser` to choose Chrome or Firefox
   - `--output` / `-o` to save results to file
   - `--json` for JSON output format
   - `--header` and `--cookie` for authenticated requests
   - `--resume` for checkpoint/resume functionality
   - `--filter` and `--exclude` for result filtering
3. Implement core route discovery logic (`hunter.py`) using Selenium WebDriver:
   - Baseline creation from base URL
   - Page information extraction (title, H1, content length)
   - Differential analysis comparing tested pages against baseline
   - Two-rule detection system: title/H1 differences OR significant content differences
4. Implement checkpoint/resume functionality for long-running scans
5. Add progress tracking and status reporting during enumeration
6. Support for both hash-based (`/#route`) and History API (`/route`) route testing

**Tool Structure:**
```
route-hunter/
├── route_hunter/
│   ├── __init__.py
│   ├── cli.py (CLI interface and argument parsing)
│   └── hunter.py (Core Selenium-based route discovery logic)
├── setup.py (Python package configuration)
├── README.md (Documentation)
└── requirements.txt (Dependencies: selenium, webdriver-manager)
```

**Observations:**
- Selenium is required to render JavaScript and execute client-side routing
- Differential analysis is more reliable than HTTP status codes for SPAs
- Baseline comparison helps avoid false positives from dynamic content
- Headless mode allows automation without visible browser windows
- Checkpoint/resume functionality is essential for large wordlists
- Supporting both hash and History API routes covers different SPA routing strategies

---

### Step 2: Installation and Configuration
**Goal:** Install Route Hunter and configure it with appropriate parameters for Juice Shop enumeration.

**Actions:**
1. Install Route Hunter as a Python package:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   pip install --upgrade pip
   pip install /path/to/route-hunter
   ```
2. Prepare a wordlist file (e.g., `wordlist.txt` with common route names like `admin`, `api`, `login`, `dashboard`, etc.)
3. Identify the target Juice Shop URL (e.g., `http://localhost:3000` or remote instance)
4. Configure Route Hunter (now available as `spa-enum` command) with command-line arguments:
   ```bash
   spa-enum http://localhost:3000 wordlist.txt --headless
   ```
5. Optionally configure output file to save results:
   ```bash
   # Plain text output
   spa-enum http://localhost:3000 wordlist.txt --headless -o results.txt
   
   # JSON output
   spa-enum http://localhost:3000 wordlist.txt --headless --json -o results.json
   ```
6. For faster scanning, reduce delay and disable retries:
   ```bash
   spa-enum http://localhost:3000 wordlist.txt --headless --delay 0.2 --retries 0
   ```

**Evidence:**
- Tool successfully installs and `spa-enum` command is available
- Wordlist file is loaded and parsed correctly
- Target URL is validated and accessible
- Headless mode is enabled (no visible browser windows)
- Selenium WebDriver initializes successfully (ChromeDriver or GeckoDriver)

---

### Step 3: Execution and Discovery
**Goal:** Execute Route Hunter against Juice Shop and identify discovered client-side routes.

**Execution:**
1. Run Route Hunter with configured parameters:
   ```bash
   spa-enum http://localhost:3000 wordlist.txt --headless
   ```
2. Monitor progress output showing:
   - Baseline creation from base URL
   - Baseline characteristics (title, H1, content length)
   - Progress updates every 100 tested URLs
   - Number of routes discovered
   - Current test status
3. Observe successful route discoveries with details:
   - Route URL (hash-based `/#route` or History API `/route`)
   - Page title
   - H1 heading
   - Content length
4. Review final results summary with statistics

**Example Output:**
```
[*] Loaded 1000 words from wordlist.txt
[*] Testing URL: http://localhost:3000
[*] Variants: hash, history
[*] Delay: 0.5s

[*] Creating baseline from http://localhost:3000...
[+] Baseline created successfully
[*] Baseline: {'title': 'OWASP Juice Shop', 'h1': 'Welcome to the OWASP Juice Shop', 'content_length': 1234}
[*] Testing 1000 words...

[*] Progress: 100/2000 URLs tested, 2 routes found
[+] Found: http://localhost:3000/#/administration
    Title: Administration
    H1: Administration Panel
[+] Found: http://localhost:3000/#/complain
    Title: Customer Feedback
    H1: Submit a Complaint

[*] Progress: 200/2000 URLs tested, 3 routes found
...

============================================================
RESULTS
============================================================
Discovered routes: 5
  - Hash-based (#): 4
  - History API (/): 1
Total URLs tested: 2000
Success rate: 0.25%
Scan duration: 1234.56s
============================================================

Discovered routes:
------------------------------------------------------------
http://localhost:3000/#/administration
http://localhost:3000/#/complain
http://localhost:3000/#/contact
http://localhost:3000/#/privacy-security
http://localhost:3000/search
```

**Result:**
- Multiple client-side routes discovered that are not detectable through traditional HTTP enumeration
- Hash-based routes identified (e.g., `/#/administration`, `/#/complain`)
- History API routes discovered (e.g., `/search`)
- Administrative interfaces and sensitive pages found
- The enumeration reveals the SPA's routing structure and potential attack vectors
- Discovered routes can be further analyzed for client-side vulnerabilities, authentication bypasses, or information disclosure

---

## Video Demonstration

### 🎥 Loom Video
**Link:** [Loom Video](https://www.loom.com/share/d6ce01e04ee44f248b0cfc3df5909540)

**Video Contents:**
- Introduction to Route Hunter tool and SPA route enumeration (0:00 - 0:17)
- Tool structure and setup demonstration (0:17 - 0:28)
- CLI configuration and command-line options (0:28 - 0:39)
- Quick start guide and basic usage (0:39 - 0:50)
- Execution demonstration and progress monitoring (0:50 - 1:03)
- Command-line arguments explanation (URL, wordlist, `--headless`) (1:03 - 1:20)
- Real-time enumeration progress and route detection (1:20 - 1:52)
- Results analysis and discovered routes (1:52 - 2:05)
- Discussion of tool development insights and future enhancements (2:05 - 2:26)
- Additional features: output file saving and token/header support for authenticated requests (2:26 - 3:02)
- Potential use cases for API testing with authentication tokens (3:02 - 3:13)

---

## Mitigation & Prevention

### How to Fix This Vulnerability

#### Developer Recommendations:
1. **Implement Proper Route Protection**
   - Implementation: Ensure all client-side routes, especially administrative or sensitive ones, require proper authentication and authorization. Use route guards in your SPA framework to protect sensitive routes.
   - Code example (React Router):
   ```javascript
   // ✅ SECURE - Protected route with authentication check
   <Route 
     path="/administration" 
     element={
       <RequireAuth>
         <AdminPanel />
       </RequireAuth>
     } 
   />
   ```

2. **Minimize Information Disclosure in Route Names**
   - Implementation: Avoid using obvious route names like `/admin`, `/api`, `/secret`. While security through obscurity isn't sufficient, it can slow down automated enumeration. More importantly, ensure routes are properly protected regardless of their names.

3. **Implement Rate Limiting and Bot Detection**
   - Implementation: Implement rate limiting and bot detection mechanisms to identify and block automated enumeration attempts. Monitor for patterns of rapid route access that indicate scanning activity.
   - Code example:
   ```javascript
   // Server-side rate limiting
   const rateLimit = require('express-rate-limit');
   const limiter = rateLimit({
     windowMs: 15 * 60 * 1000,
     max: 100
   });
   app.use(limiter);
   ```

4. **Use Consistent Page Metadata**
   - Implementation: For routes that should not be discoverable, use consistent page titles and H1 headings that match the baseline. However, this is a weak defense - proper authentication is essential.
   - Note: This makes enumeration slightly harder but doesn't prevent it entirely.

5. **Implement Proper Error Handling**
   - Implementation: Ensure that non-existent routes don't reveal information about the application structure. Use consistent 404 pages that don't differ significantly from the baseline.
   - Code example:
   ```javascript
   // ✅ SECURE - Consistent 404 page
   <Route path="*" element={<NotFound />} />
   // NotFound component should have similar structure to baseline
   ```

6. **Client-Side Route Validation**
   - Implementation: Validate routes on the server side when possible. Don't rely solely on client-side routing - ensure sensitive routes require server-side authentication checks.

#### Security Best Practices:
- Regularly perform security audits to identify and secure all exposed endpoints
- Use Web Application Firewalls (WAF) to detect and block enumeration attempts
- Implement proper logging and monitoring to detect scanning activities
- Use robots.txt appropriately (but don't rely on it for security)
- Consider using non-standard endpoint names for sensitive functionality (though this is security through obscurity)
- Implement CAPTCHA or other bot detection mechanisms for sensitive endpoints
- Use authentication tokens and API keys properly, and rotate them regularly
- Document all endpoints and maintain an API inventory

---

## Tools Used

| Tool | Purpose | Version/Link |
|------|---------|--------------|
| Route Hunter (Custom Tool) | Headless SPA route discovery tool using Selenium | Custom Python implementation |
| Selenium WebDriver | Browser automation for JavaScript rendering | [Selenium](https://www.selenium.dev/) |
| Python 3.7+ | Tool development language | [Python](https://www.python.org/) |
| Chrome/Firefox | Browser for Selenium automation | Chrome/Chromium or Firefox |
| OWASP Juice Shop | Target application for testing | [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) |
| Wordlist Files | Common route names for enumeration | Custom wordlist or [SecLists](https://github.com/danielmiessler/SecLists) |

---

## References & Further Reading

1. [OWASP Top 10 - A05:2021 Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)
2. [OWASP Testing Guide - Information Gathering](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/)
3. [Selenium WebDriver Documentation](https://www.selenium.dev/documentation/)
4. [SecLists - Collection of wordlists](https://github.com/danielmiessler/SecLists)
5. [Python Selenium Package](https://pypi.org/project/selenium/)
6. [Single Page Application Security - OWASP](https://owasp.org/www-community/vulnerabilities/Client_Side_Resource_Manipulation)

---

## Notes & Reflections

### What I Learned
- Building custom security tools provides deeper understanding of how enumeration tools work and how to defend against them
- SPA route enumeration requires different techniques than traditional directory enumeration - browser automation is essential
- Differential analysis (comparing page characteristics) is more reliable than HTTP status codes for client-side routes
- Selenium WebDriver enables JavaScript rendering and client-side route detection that traditional tools miss
- Headless mode is essential for automation and integration into security testing pipelines
- Baseline comparison helps avoid false positives from dynamic content and timing variations
- Supporting both hash-based and History API routes covers different SPA routing strategies
- CLI design and user experience are important even for security tools - clear progress reporting and output formatting matter
- Checkpoint/resume functionality is crucial for long-running scans with large wordlists
- Tool development skills are valuable for creating specialized tools tailored to specific testing scenarios
- The ability to add features like header/cookie support makes tools more versatile for authenticated testing scenarios

### Challenges Faced
- Implementing reliable baseline creation and comparison logic to avoid false positives
- Handling Selenium WebDriver initialization, crashes, and connection issues
- Designing differential analysis rules that detect real routes while filtering out timing variations
- Balancing enumeration speed (delay settings) with reliable page rendering
- Implementing checkpoint/resume functionality for long-running scans
- Handling different SPA routing strategies (hash-based vs History API)
- Managing browser resources efficiently during long enumeration sessions
- Designing a clear CLI interface that provides useful feedback without cluttering the output
- Handling edge cases like page load timeouts, JavaScript errors, and network issues
- Deciding on content difference thresholds that balance sensitivity with false positive reduction

### Additional Observations
- SPA route enumeration is fundamentally different from traditional directory enumeration - requires browser automation
- Custom tool development allows for specific feature additions (like header/cookie support for authenticated testing) that may not be available in standard tools
- Building tools from scratch provides valuable learning opportunities in browser automation, differential analysis, and security testing methodologies
- The concept of building a personal security tool collection (like a custom Metasploit framework) is an interesting long-term project idea
- Route enumeration is often the first step in penetration testing, making it a fundamental skill
- The discovered routes can reveal SPA architecture, client-side routing structure, and potential attack vectors
- Headless tools are essential for automation and CI/CD integration in DevSecOps workflows
- The ability to save results to JSON format enables further analysis, reporting, and integration with other tools
- Differential analysis is more sophisticated than HTTP status code checking but provides better results for modern SPAs
- Supporting both hash-based and History API routes ensures comprehensive coverage of different SPA frameworks

---

**⚠️ Disclaimer:** This documentation is created solely for educational purposes as part of a DevSecOps training program. All activities were performed in a controlled, legal environment (OWASP Juice Shop). Never attempt these techniques on systems you don't own or have explicit permission to test.

---

**Date Completed:** [2025-11-16]  
**Author:** [Uwe Wohlleber]

