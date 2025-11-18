# Password Strength - ⭐⭐

## Overview

**Category:** Broken Authentication / Brute Force  
**Difficulty:** ⭐⭐ (2/6)  
**OWASP Top 10:** A07:2021 – Identification and Authentication Failures

### Brief Description
This challenge demonstrates the vulnerability of weak passwords to brute-force attacks. By using a password list and automated login attempts, we can successfully authenticate to a user account with a weak password. The challenge highlights the importance of strong password policies and account lockout mechanisms to prevent brute-force attacks.

---

## Security Vulnerability Explained

### What is Brute-Force Attack?
A brute-force attack is a trial-and-error method used to obtain information such as user passwords or personal identification numbers (PINs). In this attack, an attacker systematically checks all possible passwords and passphrases until the correct one is found. When targeting authentication systems, attackers use automated tools to rapidly submit many password guesses, exploiting accounts with weak or common passwords.

### Dangers and Risks
Brute-force attacks pose significant security risks to web applications and their users:

- **Unauthorized Access:** Attackers can gain access to user accounts, potentially accessing sensitive personal information, financial data, or private communications.
- **Account Takeover:** Once an attacker successfully brute-forces a password, they can take full control of the account, change passwords, modify account settings, and perform actions on behalf of the legitimate user.
- **Credential Stuffing:** Compromised passwords are often reused across multiple services, allowing attackers to access other accounts using the same credentials.
- **Business Consequences:** 
  - Data breaches leading to regulatory fines (GDPR, CCPA)
  - Loss of customer trust and reputation damage
  - Financial losses from fraud and account recovery costs
  - Legal liability for failing to protect user accounts
- **Real-world Examples:**
  - *2012 LinkedIn breach:* 6.5 million passwords cracked using brute-force techniques
  - *2019 Collection #1-5 breaches:* Billions of credentials exposed and used in credential stuffing attacks

### Why This Matters
Weak passwords and lack of brute-force protection are among the most common security vulnerabilities in web applications. Many users still choose simple, predictable passwords that can be easily guessed or cracked. Without proper security controls like account lockout mechanisms, rate limiting, or CAPTCHA challenges, applications are vulnerable to automated brute-force attacks. Understanding these vulnerabilities helps developers implement proper authentication security measures.

---

## Challenge Documentation

### Challenge Description
The "Password Strength" challenge requires finding a user account with a weak password that can be discovered through a brute-force attack. The application allows multiple login attempts without implementing proper rate limiting or account lockout mechanisms. By using a password list and an automated script to systematically test passwords, we can successfully authenticate to an account with a weak password. This demonstrates the critical importance of strong password policies and brute-force protection mechanisms.

### Prerequisites
- [x] OWASP Juice Shop application running locally or remotely
- [x] Burp Suite Community/Professional Edition installed
- [x] Python 3 installed with `requests` library
- [x] Password list file (e.g., `password-list.txt`)
- [x] Basic understanding of HTTP requests and authentication mechanisms
- [x] Knowledge of brute-force attack techniques

---

## Exploitation Steps

### Step 1: Reconnaissance
**Goal:** Identify the target user's email address and capture the authentication request structure.

**Actions:**
1. Log in to the Juice Shop application to identify the target user account (`admin@juice-sh.op`)
2. Navigate to the Juice Shop login page
3. Open Burp Suite and configure the browser proxy settings
4. Enable Burp Interceptor to capture HTTP requests
5. Attempt a normal login to capture the request structure
6. Copy the complete HTTP request from Burp Suite (including all headers and the JSON body)

**Observations:**
- The target user's email address is identified: `admin@juice-sh.op`
- The login form sends a POST request to `/rest/user/login`
- The request contains email and password parameters in JSON format
- The request includes various headers (User-Agent, Content-Type, Cookies, etc.)
- The complete request structure is captured for use in the brute-force script
- The application does not appear to implement rate limiting or account lockout after failed attempts

---

### Step 2: Vulnerability Identification
**Goal:** Confirm that the application is vulnerable to brute-force attacks by analyzing the login mechanism.

**Actions:**
1. Capture the login request using Burp Suite Interceptor
2. Copy the complete HTTP request (including headers and body)
3. Analyze the request structure to understand the authentication flow
4. Test multiple login attempts to verify lack of rate limiting

**Evidence:**
- The captured request shows email and password parameters being sent to the server
- Multiple failed login attempts do not trigger account lockout
- No CAPTCHA or rate limiting is implemented
- The application accepts rapid successive login attempts
- Error responses indicate whether credentials are invalid but don't prevent further attempts

---

### Step 3: Exploitation
**Goal:** Successfully authenticate to a user account by brute-forcing the password using a password list.

**Payload/Technique:**
Create a Python script that:
1. Reads passwords from a password list file
2. Sends POST requests to the login endpoint with each password
3. Monitors responses to identify successful authentication
4. Stops when a valid password is found

**Execution:**
1. Capture the login request from Burp Suite and copy the complete request structure:
   ```
   POST /rest/user/login HTTP/1.1
   Host: 10.20.20.54:3000
   Content-Type: application/json
   ...
   
   {"email":"admin@juice-sh.op","password":"test"}
   ```
   - This request structure is used as a template for the brute-force script
   - All headers and the JSON format are preserved

2. Create a Python script (`login_request.py`) that:
   - Uses the captured request structure from Burp Suite
   - Accepts user email via `-u` flag
   - Accepts password list file via `-p` flag
   - Replaces the password in the request with each password from the list
   - Sends the request with the specified email and each password sequentially

3. Prepare a password list file (`password-list.txt`) containing common passwords:
   ```
   password
   123456
   admin
   qwerty
   ...
   ```

4. Execute the brute-force script with the target user and password list:
   ```bash
   python3 login_request.py -u admin@juice-sh.op -p password-list.txt
   ```
   - The `-u` flag specifies the target user's email address
   - The `-p` flag specifies the password list file to use

5. The script will:
   - Load passwords from the list file
   - For each password, send a login request using the captured request structure
   - Replace only the password field while keeping all headers and other parameters
   - Display progress for each attempt (e.g., `[1/100] Trying password: ...`)
   - Stop immediately when a successful login is detected (HTTP 200/201 status)
   - Display the successful password and authentication details

**Result:**
- ✅ Successfully authenticated with a weak password from the list
- Session token/JWT issued for the user account
- Challenge solved notification appears
- Access to the user account with the compromised credentials

---

## Video Demonstration

### 🎥 Loom Video
**Link:** [Loom Video](https://www.loom.com/share/ef76e0596fdf4bb4b9a08e5406bccc63)

**Video Contents:**
- Introduction to the Password Strength challenge (0:00 - 0:01)
  - Overview of the brute-force attack approach
- Identifying the target user (0:01 - 0:28)
  - Logging in to identify the email address (`admin@juice-sh.op`)
  - Understanding the target account
- Building the brute-force script (0:28 - 0:39)
  - Creating a Python script that accepts user and password list parameters
  - Script functionality: tests passwords sequentially until a match is found
- Capturing the login request from Burp Suite (0:39 - 1:02)
  - Copying the complete login request structure
  - Understanding the request format (headers, JSON body)
  - Using the request as a template for the script
  - Script replaces the password field while keeping all other request details
- Executing the brute-force attack (1:02 - 1:25)
  - Running the script with `-u` flag for user (email address)
  - Running the script with `-p` flag for password list file
  - Script automatically tests each password from the list
  - Observing the script progress through password attempts
- Successful authentication (1:25 - 1:33)
  - Script identifies the correct password
  - Displaying the successful match
  - Challenge completion

---

## Mitigation & Prevention

### How to Fix This Vulnerability

#### Developer Recommendations:

1. **Implement Account Lockout Mechanisms**
   - Implementation: Lock accounts after a specified number of failed login attempts (e.g., 5 attempts) for a certain duration (e.g., 15-30 minutes).
   - Code example:
   ```javascript
   // ✅ SECURE - Account lockout implementation
   const MAX_LOGIN_ATTEMPTS = 5;
   const LOCKOUT_DURATION = 30 * 60 * 1000; // 30 minutes
   
   async function checkAccountLockout(email) {
     const user = await db.users.findOne({ where: { email } });
     if (user.failedLoginAttempts >= MAX_LOGIN_ATTEMPTS) {
       const lockoutTime = new Date(user.lastFailedLogin);
       const now = new Date();
       if (now - lockoutTime < LOCKOUT_DURATION) {
         throw new Error('Account locked. Please try again later.');
       } else {
         // Reset attempts after lockout period
         await user.update({ failedLoginAttempts: 0 });
       }
     }
   }
   ```

2. **Implement Rate Limiting**
   - Implementation: Limit the number of login requests per IP address or per account within a time window.
   - Code example:
   ```javascript
   // ✅ SECURE - Rate limiting with express-rate-limit
   const rateLimit = require('express-rate-limit');
   
   const loginLimiter = rateLimit({
     windowMs: 15 * 60 * 1000, // 15 minutes
     max: 5, // Limit each IP to 5 requests per windowMs
     message: 'Too many login attempts, please try again later.',
     standardHeaders: true,
     legacyHeaders: false,
   });
   
   app.post('/rest/user/login', loginLimiter, async (req, res) => {
     // Login logic...
   });
   ```

3. **Enforce Strong Password Policies**
   - Implementation: Require passwords to meet complexity requirements (minimum length, mixed case, numbers, special characters).
   - Code example:
   ```javascript
   // ✅ SECURE - Password strength validation
   function validatePasswordStrength(password) {
     const minLength = 12;
     const hasUpperCase = /[A-Z]/.test(password);
     const hasLowerCase = /[a-z]/.test(password);
     const hasNumbers = /\d/.test(password);
     const hasSpecialChar = /[!@#$%^&*(),.?":{}|<>]/.test(password);
     
     if (password.length < minLength) {
       throw new Error(`Password must be at least ${minLength} characters long`);
     }
     if (!hasUpperCase || !hasLowerCase) {
       throw new Error('Password must contain both uppercase and lowercase letters');
     }
     if (!hasNumbers) {
       throw new Error('Password must contain at least one number');
     }
     if (!hasSpecialChar) {
       throw new Error('Password must contain at least one special character');
     }
     
     // Check against common password list
     const commonPasswords = require('./common-passwords.json');
     if (commonPasswords.includes(password.toLowerCase())) {
       throw new Error('Password is too common. Please choose a stronger password.');
     }
   }
   ```

4. **Implement CAPTCHA Challenges**
   - Implementation: Require CAPTCHA verification after a few failed login attempts to prevent automated attacks.
   - Code example:
   ```javascript
   // ✅ SECURE - CAPTCHA after failed attempts
   app.post('/rest/user/login', async (req, res) => {
     const { email, password, captchaToken } = req.body;
     const user = await db.users.findOne({ where: { email } });
     
     if (user.failedLoginAttempts >= 3) {
       if (!captchaToken) {
         return res.status(400).json({ 
           error: 'CAPTCHA required',
           requiresCaptcha: true 
         });
       }
       // Verify CAPTCHA token with service (e.g., Google reCAPTCHA)
       const captchaValid = await verifyCaptcha(captchaToken);
       if (!captchaValid) {
         return res.status(400).json({ error: 'Invalid CAPTCHA' });
       }
     }
     
     // Continue with login...
   });
   ```

5. **Use Multi-Factor Authentication (MFA)**
   - Implementation: Require additional authentication factors (SMS, email, authenticator app) for sensitive accounts.
   - Code example:
   ```javascript
   // ✅ SECURE - MFA implementation
   app.post('/rest/user/login', async (req, res) => {
     const { email, password, mfaCode } = req.body;
     
     // Verify password first
     const user = await db.users.findOne({ where: { email } });
     if (!user || !await bcrypt.compare(password, user.passwordHash)) {
       return res.status(401).json({ error: 'Invalid credentials' });
     }
     
     // Check if MFA is enabled
     if (user.mfaEnabled) {
       if (!mfaCode) {
         // Send MFA code and request it
         await sendMFACode(user);
         return res.status(200).json({ requiresMFA: true });
       }
       
       // Verify MFA code
       if (!await verifyMFACode(user, mfaCode)) {
         return res.status(401).json({ error: 'Invalid MFA code' });
       }
     }
     
     // Generate session token...
   });
   ```

6. **Implement Progressive Delays**
   - Implementation: Increase delay between login attempts exponentially with each failed attempt.
   - Code example:
   ```javascript
   // ✅ SECURE - Progressive delay
   async function handleFailedLogin(email) {
     const user = await db.users.findOne({ where: { email } });
     const attempts = user.failedLoginAttempts + 1;
     
     await user.update({
       failedLoginAttempts: attempts,
       lastFailedLogin: new Date()
     });
     
     // Progressive delay: 2^attempts seconds
     const delaySeconds = Math.min(Math.pow(2, attempts), 60); // Max 60 seconds
     await new Promise(resolve => setTimeout(resolve, delaySeconds * 1000));
   }
   ```

#### Security Best Practices:
- Enforce strong password policies (minimum 12 characters, complexity requirements)
- Implement account lockout after failed attempts
- Use rate limiting to prevent rapid automated attacks
- Require CAPTCHA after multiple failed attempts
- Implement multi-factor authentication for sensitive accounts
- Monitor and log all login attempts for security analysis
- Use secure password hashing (bcrypt, argon2) with appropriate cost factors
- Educate users about password security and password managers
- Regularly check user passwords against known breach databases
- Implement progressive delays for failed login attempts

---

## Tools Used

| Tool | Purpose | Version/Link |
|------|---------|--------------|
| Burp Suite | Capture and analyze HTTP requests | Community/Professional Edition |
| Python 3 | Scripting language for brute-force automation | Python 3.x |
| requests library | HTTP library for making login requests | `pip install requests` |
| password-list.txt | Common passwords list for brute-force | Custom/Public wordlists |

---

## References & Further Reading

1. [OWASP - Brute Force Attack](https://owasp.org/www-community/attacks/Brute_force_attack)
2. [OWASP - Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
3. [OWASP - Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
4. [NIST Password Guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html)
5. [OWASP Top 10 - A07:2021 Identification and Authentication Failures](https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/)

---

## Notes & Reflections

### What I Learned
This challenge demonstrated how easily weak passwords can be compromised through automated brute-force attacks. The lack of rate limiting and account lockout mechanisms made it trivial to test hundreds of passwords in a short time. The experience highlighted the importance of implementing multiple layers of security controls, including strong password policies, rate limiting, and account lockout mechanisms.

### Challenges Faced
The main challenge was creating an effective brute-force script that could handle the HTTP request format correctly, including all necessary headers and cookies. Understanding the exact request structure from Burp Suite was crucial for successful automation. Additionally, ensuring the script properly identified successful authentication responses was important to avoid false positives.

### Additional Observations
- The application's error messages were consistent, making it easy to distinguish between failed and successful login attempts
- The lack of any rate limiting or CAPTCHA made the brute-force attack very straightforward
- Common password lists are highly effective against accounts with weak passwords
- The challenge emphasizes the need for both technical controls (rate limiting, lockout) and user education (strong passwords)

---

**⚠️ Disclaimer:** This documentation is created solely for educational purposes as part of a DevSecOps training program. All activities were performed in a controlled, legal environment (OWASP Juice Shop). Never attempt these techniques on systems you don't own or have explicit permission to test.

---

**Date Completed:** 2025-11-16  
**Author:** Uwe Wohlleber

