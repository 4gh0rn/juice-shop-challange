# [Empty User Registration] - [Difficulty Level ⭐⭐]

## Overview

**Category:** [Input Validation / Broken Authentication]  
**Difficulty:** [2 stars]  
**OWASP Top 10:** A07:2021 – Identification and Authentication Failures / A03:2021 – Injection

### Brief Description
This challenge involves exploiting a lack of server-side input validation in the user registration and login processes. By intercepting the registration and login requests with Burp Suite and removing or emptying all required fields (email, password, etc.), we can successfully register and log in with empty credentials. This demonstrates how relying solely on client-side validation allows attackers to bypass security controls and create accounts or authenticate without providing valid credentials.

---

## Security Vulnerability Explained

### What is Missing Input Validation?
Missing or insufficient input validation occurs when an application fails to properly validate user input on the server side. While client-side validation (JavaScript in the browser) can improve user experience, it can always be bypassed by manipulating HTTP requests. When server-side validation is missing or incomplete, attackers can submit empty, malformed, or malicious input that bypasses intended security controls. In this case, the application accepts empty registration and login credentials because it only validates input on the client side, which can be easily bypassed using tools like Burp Suite.

### Dangers and Risks
Missing server-side input validation exposes critical security risks:

- **Unauthorized Access:** Attackers can create accounts or authenticate without providing valid credentials, bypassing authentication mechanisms entirely.
- **Data Integrity Issues:** Empty or invalid data can corrupt the database, cause application errors, or create inconsistent system states.
- **Privilege Escalation:** Bypassing validation might allow attackers to access features or data they shouldn't have access to.
- **Business Logic Bypass:** Missing validation can allow attackers to bypass business rules, such as required fields, password complexity, or email verification.
- **System Instability:** Invalid or empty data can cause application crashes, database errors, or unexpected behavior.
- **Real-world Examples:**
  - *Multiple web applications:* Missing server-side validation has allowed attackers to create accounts with empty credentials, bypassing authentication.
  - *E-commerce platforms:* Insufficient validation has enabled attackers to place orders with invalid or empty payment information.

### Why This Matters
Input validation is a fundamental security control that must be implemented on the server side. Client-side validation is easily bypassed and should only be used for user experience improvements, never for security. Missing server-side validation is one of the most common vulnerabilities in web applications and can lead to serious security breaches. Understanding how to identify and exploit missing validation helps developers recognize the importance of implementing proper server-side checks for all user input.

---

## Challenge Documentation

### Challenge Description
The "Empty User Registration" challenge requires exploiting missing server-side input validation in both the registration and login processes. The application appears to validate user input only on the client side (in the browser), which can be bypassed by intercepting and modifying HTTP requests. By using Burp Suite to capture the registration request, removing or emptying all fields (email, password, etc.), and forwarding the modified request, we can successfully register a user with empty credentials. Similarly, by modifying the login request to use empty fields, we can authenticate without providing valid credentials. This demonstrates the critical importance of server-side validation.

### Prerequisites
- [x] OWASP Juice Shop application running locally or remotely
- [x] Burp Suite Community/Professional Edition installed
- [x] Browser configured to use Burp Suite as proxy
- [x] Basic understanding of HTTP requests and request manipulation
- [x] Knowledge of how web applications handle registration and authentication

---

## Exploitation Steps

### Step 1: Reconnaissance
**Goal:** Identify the registration and login endpoints and understand the request structure.

**Actions:**
1. Navigate to the Juice Shop registration page
2. Open Burp Suite and configure the browser proxy settings
3. Enable Burp Interceptor to capture HTTP requests
4. Attempt a normal registration to capture the request structure
5. Observe the registration request:
   - HTTP method (likely POST)
   - Endpoint URL (e.g., `/api/Users` or `/rest/user/registration`)
   - Request body structure (JSON with email, password, etc.)
   - Required fields

**Observations:**
- The registration form has fields for email, password, and possibly other information
- Client-side validation prevents submitting empty fields in the browser
- The registration request is sent as a POST request with JSON body
- The request can be intercepted and modified in Burp Suite

---

### Step 2: Vulnerability Identification
**Goal:** Confirm that server-side validation is missing by analyzing the request structure.

**Actions:**
1. Review the captured registration request structure
2. Analyze the request body:
   - Identify all fields in the request body (email, password, passwordRepeat, etc.)
   - Note the JSON structure
3. Observe that client-side validation prevents empty submissions in the browser
4. Note that the request can be intercepted and modified in Burp Suite

**Evidence:**
- The registration request contains fields that should be validated server-side
- Client-side validation can be bypassed by intercepting the request
- No visible server-side validation is apparent from the request structure
- This suggests the vulnerability may exist

---

### Step 3: Exploitation - Registration
**Goal:** Successfully register a user with empty credentials.

**Payload/Technique:**
```http
POST /api/Users HTTP/1.1
Host: localhost:3000
Content-Type: application/json

{
  "email": "",
  "password": "",
  "passwordRepeat": ""
}
```

Or by removing the fields entirely:
```http
POST /api/Users HTTP/1.1
Host: localhost:3000
Content-Type: application/json

{}
```

**Execution:**
1. Navigate to the registration page in the browser
2. Enable Burp Suite Interceptor to capture requests
3. Fill in the registration form with valid-looking data (e.g., email, password) - these will be emptied in the intercepted request
4. Submit the registration form
5. When the POST request is intercepted in Burp Suite:
   - Modify the request body to empty all fields
   - Set email to `""` (empty string)
   - Set password to `""` (empty string)
   - Set passwordRepeat to `""` (empty string)
   - Or remove the fields entirely from the JSON (leaving `{}`)
6. Forward the modified request
7. Observe the successful registration response

**Result:**
- Registration succeeds with empty credentials
- A user account is created with empty email and password fields
- The application confirms successful registration
- The created user has no username or other identifying information
- This demonstrates that server-side validation is completely missing

---

### Step 4: Exploitation - Login
**Goal:** Successfully log in using the empty credentials.

**Payload/Technique:**
```http
POST /rest/user/login HTTP/1.1
Host: localhost:3000
Content-Type: application/json

{
  "email": "",
  "password": ""
}
```

**Execution:**
Follow the same process as Step 3, but for the login endpoint:
1. Navigate to the login page
2. Enable Burp Suite Interceptor
3. Enter any values in the login form
4. Click "Log in"
5. When the request is intercepted, modify the request body to set both `email` and `password` to empty strings (`""`)
6. Forward the modified request

**Result:**
- Login succeeds with empty credentials
- Authentication token is received
- The user is successfully logged in with the empty account created in Step 3
- The user profile shows no username or identifying information (empty fields)
- The challenge is marked as completed
- This confirms that both registration and login lack proper server-side validation

---

## Video Demonstration

### 🎥 Loom Video
**Link:** [Loom Video](https://www.loom.com/share/8f4b7728e4ad4a039ef589dc9c5521a1)

**Video Contents:**
- Introduction to the Empty User Registration challenge (0:00 - 0:11)
- Navigating to Juice Shop and setting up Burp Suite Interceptor (0:11 - 0:19)
- Entering valid-looking data in the registration form (0:19 - 0:39)
- Capturing the registration POST request with Burp Suite (0:39 - 0:57)
- Modifying the registration request to empty all fields (0:57 - 1:08)
- Successful registration with empty credentials (1:08 - 1:36)
- Demonstrating the same technique for login:
  - Entering data in the login form (1:36 - 1:50)
  - Capturing the login POST request (1:50 - 1:59)
  - Modifying the login request to use empty email and password
  - Forwarding the request and successfully logging in
- Showing the logged-in user with no username/empty profile (1:59 - end)

---

## Mitigation & Prevention

### How to Fix This Vulnerability

#### Developer Recommendations:
1. **Implement Server-Side Validation**
   - Implementation: Always validate all user input on the server side, regardless of client-side validation. Check for required fields, data types, formats, and constraints.
   - Code example:
   ```javascript
   // ❌ VULNERABLE - No server-side validation
   app.post('/api/Users', (req, res) => {
     const user = req.body;
     db.users.create(user);
     res.json({ success: true });
   });
   
   // ✅ SECURE - Server-side validation
   const { body, validationResult } = require('express-validator');
   
   app.post('/api/Users', [
     body('email').isEmail().notEmpty().withMessage('Email is required and must be valid'),
     body('password').isLength({ min: 8 }).notEmpty().withMessage('Password is required and must be at least 8 characters'),
     body('passwordRepeat').custom((value, { req }) => {
       if (value !== req.body.password) {
         throw new Error('Passwords do not match');
       }
       return true;
     })
   ], (req, res) => {
     const errors = validationResult(req);
     if (!errors.isEmpty()) {
       return res.status(400).json({ errors: errors.array() });
     }
     
     const user = req.body;
     db.users.create(user);
     res.json({ success: true });
   });
   ```

2. **Validate All Required Fields**
   - Implementation: Explicitly check that all required fields are present and not empty before processing the request.
   - Code example:
   ```javascript
   // ✅ SECURE - Explicit required field validation
   function validateRegistration(user) {
     if (!user.email || user.email.trim() === '') {
       throw new Error('Email is required');
     }
     if (!user.password || user.password.trim() === '') {
       throw new Error('Password is required');
     }
     if (user.password.length < 8) {
       throw new Error('Password must be at least 8 characters');
     }
     // Additional validations...
     return true;
   }
   ```

3. **Use Validation Libraries**
   - Implementation: Use established validation libraries (like express-validator for Node.js, Joi, or class-validator) that provide comprehensive validation rules and error handling.
   - Code example:
   ```javascript
   // ✅ SECURE - Using Joi validation library
   const Joi = require('joi');
   
   const registrationSchema = Joi.object({
     email: Joi.string().email().required(),
     password: Joi.string().min(8).required(),
     passwordRepeat: Joi.string().valid(Joi.ref('password')).required()
   });
   
   app.post('/api/Users', (req, res) => {
     const { error, value } = registrationSchema.validate(req.body);
     if (error) {
       return res.status(400).json({ error: error.details[0].message });
     }
     // Process valid data...
   });
   ```

4. **Sanitize Input Data**
   - Implementation: In addition to validation, sanitize input data to remove potentially dangerous characters and normalize data formats.
   - Code example:
   ```javascript
   // ✅ SECURE - Sanitize and validate
   const validator = require('validator');
   
   function sanitizeAndValidate(user) {
     const sanitized = {
       email: validator.normalizeEmail(user.email),
       password: user.password.trim()
     };
     
     if (!validator.isEmail(sanitized.email)) {
       throw new Error('Invalid email format');
     }
     
     return sanitized;
   }
   ```

5. **Implement Authentication Checks**
   - Implementation: For login, verify that credentials are not empty and match existing user records. Never authenticate users with empty credentials.
   - Code example:
   ```javascript
   // ✅ SECURE - Proper authentication validation
   app.post('/rest/user/login', async (req, res) => {
     const { email, password } = req.body;
     
     // Validate input
     if (!email || email.trim() === '') {
       return res.status(400).json({ error: 'Email is required' });
     }
     if (!password || password.trim() === '') {
       return res.status(400).json({ error: 'Password is required' });
     }
     
     // Find user and verify password
     const user = await db.users.findOne({ where: { email } });
     if (!user || !await bcrypt.compare(password, user.passwordHash)) {
       return res.status(401).json({ error: 'Invalid credentials' });
     }
     
     // Generate token...
   });
   ```

#### Security Best Practices:
- Always validate input on the server side - never trust client-side validation alone
- Use established validation libraries and frameworks
- Validate data types, formats, lengths, and constraints
- Check for required fields explicitly
- Sanitize input data to prevent injection attacks
- Implement proper error handling that doesn't reveal system internals
- Use parameterized queries to prevent SQL injection
- Implement rate limiting on registration and login endpoints
- Log validation failures for security monitoring
- Regularly audit validation logic for completeness

---

## Tools Used

| Tool | Purpose | Version/Link |
|------|---------|--------------|
| Burp Suite | HTTP request interception and manipulation | [Burp Suite Community](https://portswigger.net/burp/communitydownload) |
| Burp Interceptor | Capturing and modifying requests in real-time | Included in Burp Suite |
| OWASP Juice Shop | Target application for testing | [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) |

---

## References & Further Reading

1. [OWASP Top 10 - A07:2021 Identification and Authentication Failures](https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/)
2. [OWASP - Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
3. [OWASP - Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
4. [Burp Suite Documentation](https://portswigger.net/burp/documentation)
5. [OWASP Juice Shop Documentation](https://pwning.owasp-juice.shop/)
6. [Express Validator Documentation](https://express-validator.github.io/docs/)

---

## Notes & Reflections

### What I Learned
- Client-side validation can always be bypassed and should never be relied upon for security
- Server-side validation is essential for all user input, especially for authentication and registration
- Burp Suite makes it easy to intercept and modify HTTP requests, bypassing client-side checks
- Missing server-side validation is a common vulnerability that can lead to serious security issues
- Empty input validation is often overlooked but can be easily exploited
- Understanding request manipulation is crucial for security testing
- Both registration and login processes must have proper server-side validation

### Challenges Faced
- Identifying which fields to empty in the registration request
- Understanding the JSON structure of the requests
- Ensuring the modified request format is correct
- Confirming that the server accepts empty values
- Testing both registration and login with empty credentials

### Additional Observations
- The vulnerability demonstrates how critical server-side validation is
- This type of vulnerability is often introduced when developers rely too heavily on client-side validation
- Empty input validation is a basic security control that should always be implemented
- The exploit is simple but demonstrates a fundamental security flaw
- This vulnerability can lead to unauthorized access and data integrity issues
- Proper validation should be implemented at multiple layers (input validation, business logic, database constraints)

---

**⚠️ Disclaimer:** This documentation is created solely for educational purposes as part of a DevSecOps training program. All activities were performed in a controlled, legal environment (OWASP Juice Shop). Never attempt these techniques on systems you don't own or have explicit permission to test.

---

**Date Completed:** 2025-11-16  
**Author:** Uwe Wohlleber

