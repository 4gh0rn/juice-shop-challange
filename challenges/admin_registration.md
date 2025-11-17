# Admin Registration - ⭐⭐⭐

## Overview

**Category:** Privilege Escalation / Broken Access Control  
**Difficulty:** ⭐⭐⭐ (3/6)  
**OWASP Top 10:** A01:2021 – Broken Access Control

### Brief Description
This challenge demonstrates how broken access control allows attackers to escalate privileges during user registration. By intercepting the registration request with Burp Suite and adding a `role` attribute set to `"admin"`, we can create a new user account with administrator privileges. This vulnerability occurs when the application trusts client-side input and fails to validate that users cannot assign themselves privileged roles during registration.

---

## Security Vulnerability Explained

### What is Privilege Escalation?
Privilege escalation is a security vulnerability that allows users to gain access to resources or permissions beyond what they are authorized to have. In web applications, this often occurs when users can manipulate requests to assign themselves administrative roles or access privileged functionality. Broken access control vulnerabilities enable attackers to bypass authorization checks and perform actions they shouldn't be able to perform, such as creating admin accounts, accessing other users' data, or modifying system settings.

### Dangers and Risks
Privilege escalation and broken access control pose severe security risks:

- **Unauthorized Administrative Access:** Attackers can gain full administrative control over the application, allowing them to modify settings, access all user data, and perform administrative actions.
- **Data Breach:** Administrative access can lead to complete data exposure, including sensitive user information, payment details, and business data.
- **System Compromise:** Attackers with admin privileges can modify application behavior, inject malicious code, or compromise the entire system.
- **Business Logic Manipulation:** Administrative access allows attackers to bypass business rules, modify pricing, grant unauthorized discounts, or manipulate order processing.
- **User Account Compromise:** Attackers can access, modify, or delete any user account, leading to identity theft and fraud.
- **Business Consequences:**
  - Complete system compromise and data breaches
  - Regulatory fines for security failures (GDPR, CCPA)
  - Loss of customer trust and reputation damage
  - Legal liability for security incidents
  - Financial losses from fraud and system recovery
- **Real-world Examples:**
  - *2018 Facebook breach:* Broken access control allowed attackers to access 50 million user accounts
  - *2019 Capital One breach:* Privilege escalation led to exposure of 100 million customer records
  - *Multiple CMS platforms:* Broken access control has allowed attackers to gain admin access

### Why This Matters
Broken access control is the most common security vulnerability in web applications, ranking #1 in the OWASP Top 10. Applications must never trust client-side input for authorization decisions. All role assignments and privilege checks must be performed on the server side, and users should never be able to assign themselves administrative roles. Understanding how to identify and exploit broken access control helps developers implement proper authorization checks and role-based access control (RBAC) mechanisms.

---

## Challenge Documentation

### Challenge Description
The "Admin Registration" challenge requires exploiting broken access control in the user registration process. By examining the application's response structure, we discover that user accounts have a `role` attribute that can be set to `"admin"`. Using Burp Suite to intercept the registration request, we can manipulate the request body to include `"role": "admin"`, creating a new user account with administrative privileges. This demonstrates how trusting client-side input for authorization decisions allows attackers to escalate privileges and gain unauthorized administrative access.

### Prerequisites
- [x] OWASP Juice Shop application running locally or remotely
- [x] Burp Suite Community/Professional Edition installed
- [x] Browser configured to use Burp Suite as proxy
- [x] Basic understanding of HTTP requests and request manipulation
- [x] Knowledge of user registration mechanisms and role-based access control

---

## Exploitation Steps

### Step 1: Reconnaissance
**Goal:** Discover that user accounts have a role attribute and understand the registration process.

**Actions:**
1. Navigate to the Juice Shop application
2. Examine user responses or API endpoints to understand the user object structure
3. Look for user profile information or API responses that show user roles
4. Identify that users have a `role` attribute that can be set to `"admin"`

**Observations:**
- User objects in API responses contain a `role` attribute
- The role attribute can have values like `"customer"` or `"admin"`
- The registration form appears to be a standard user registration interface
- No visible option to select a role during normal registration

---

### Step 2: Vulnerability Identification
**Goal:** Confirm that the registration endpoint accepts a role parameter that can be manipulated.

**Actions:**
1. Navigate to the registration page
2. Open Burp Suite and configure the browser proxy settings
3. Enable Burp Interceptor to capture HTTP requests
4. Fill out the registration form with test data:
   - Email: `uwe2@example.com` (or similar)
   - Password: `password123` (or any password)
   - Other required fields
5. Observe the registration request structure before submitting

**Evidence:**
- The registration request is sent as a POST request
- The request body contains user registration data (email, password, etc.)
- The request can be intercepted and modified in Burp Suite
- No server-side validation appears to prevent role manipulation

---

### Step 3: Exploitation
**Goal:** Manipulate the registration request to assign admin role and create an administrative account.

**Payload/Technique:**
Add the `role` attribute to the registration request body:
```json
{
  "email": "uwe2@example.com",
  "password": "password123",
  "passwordRepeat": "password123",
  "role": "admin"
}
```

**Execution:**
1. Navigate to the registration page in the browser
2. Fill out the registration form with your desired credentials:
   - Email: `uwe2@example.com`
   - Password: `password123`
   - Password Repeat: `password123`
   - Any other required fields
3. Enable Burp Suite Interceptor
4. Click the "Register" button
5. When the registration request is intercepted in Burp Suite:
   - Locate the request body (JSON format)
   - Add the `role` field: `"role": "admin"`
   - The request body should now include the role parameter
6. Forward the modified request
7. Observe the successful registration response

**Result:**
- ✅ Registration succeeds with the admin role
- A new user account is created with administrative privileges
- The application confirms successful registration
- The user account now has the `role: "admin"` attribute

---

### Step 4: Verification
**Goal:** Confirm that the newly created account has administrative privileges.

**Actions:**
1. Log out of any current session (if logged in)
2. Navigate to the login page
3. Log in with the newly created admin account:
   - Email: `uwe2@example.com`
   - Password: `password123`
4. After successful login, navigate to the admin area
5. Verify that you have access to administrative features
6. Check the user profile to confirm the role is set to "admin"

**Result:**
- ✅ Successfully logged in with the admin account
- Access to the admin area is granted
- User profile shows "Admin" as the role
- Administrative features are accessible
- Challenge is marked as completed

---

## Video Demonstration

### 🎥 Loom Video
**Link:** [Loom Video](https://www.loom.com/share/8ff082c6c0294bf596a040a2671c4c84)

**Video Contents:**
- Introduction to Admin Registration challenge (0:00 - 0:14)
  - Overview of the challenge
  - Demonstrating existing admin account access
  - Showing admin area access
- Discovering the role attribute (0:14 - 0:38)
  - Examining user responses
  - Finding the `role` attribute in API responses
  - Understanding that role can be set to "admin"
- Setting up the registration (0:38 - 1:04)
  - Navigating to registration form
  - Preparing to register a new user (e.g., user 100)
  - Entering registration details (email, password)
- Intercepting the registration request (1:04 - 1:34)
  - Enabling Burp Suite Interceptor
  - Clicking "Register" button
  - Capturing the registration request
- Manipulating the request (1:34 - 2:04)
  - Modifying the request body in Burp Suite
  - Adding `"role": "admin"` to the request
  - Forwarding the modified request
  - Successful registration with admin role
- Verifying admin access (2:04 - 2:19)
  - Logging in with the new admin account
  - Accessing admin area
  - Confirming role is set to "Admin"
  - Challenge completion

---

## Mitigation & Prevention

### How to Fix This Vulnerability

#### Developer Recommendations:

1. **Never Trust Client-Side Input for Authorization**
   - Implementation: Always assign roles on the server side. Never accept role parameters from client requests during registration.
   - Code example:
   ```javascript
   // ❌ VULNERABLE - Accepts role from client
   app.post('/api/Users', (req, res) => {
     const user = {
       email: req.body.email,
       password: req.body.password,
       role: req.body.role // DANGEROUS!
     };
     db.users.create(user);
   });
   
   // ✅ SECURE - Assign role on server side
   app.post('/api/Users', (req, res) => {
     const user = {
       email: req.body.email,
       password: hashPassword(req.body.password),
       role: 'customer' // Always default to customer
     };
     db.users.create(user);
   });
   ```

2. **Implement Role-Based Access Control (RBAC)**
   - Implementation: Use a proper RBAC system that enforces role assignments based on server-side logic and authorization checks.
   - Code example:
   ```javascript
   // ✅ SECURE - RBAC implementation
   const ROLES = {
     CUSTOMER: 'customer',
     ADMIN: 'admin'
   };
   
   function createUser(userData, createdBy) {
     // Only existing admins can create admin users
     if (userData.role === ROLES.ADMIN) {
       const creator = getUser(createdBy);
       if (creator.role !== ROLES.ADMIN) {
         throw new Error('Unauthorized: Only admins can create admin users');
       }
     }
     
     // Default to customer role
     const newUser = {
       ...userData,
       role: userData.role || ROLES.CUSTOMER
     };
     
     return db.users.create(newUser);
   }
   ```

3. **Validate and Sanitize All Input**
   - Implementation: Validate all registration input and explicitly whitelist allowed fields.
   - Code example:
   ```javascript
   // ✅ SECURE - Input validation
   const { body, validationResult } = require('express-validator');
   
   app.post('/api/Users', [
     body('email').isEmail().normalizeEmail(),
     body('password').isLength({ min: 8 }),
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
     
     // Only allow specific fields
     const allowedFields = ['email', 'password'];
     const userData = {};
     allowedFields.forEach(field => {
       if (req.body[field]) {
         userData[field] = req.body[field];
       }
     });
     
     // Role is always set server-side
     userData.role = 'customer';
     
     db.users.create(userData);
   });
   ```

4. **Use Object Property Filtering**
   - Implementation: Explicitly filter out unauthorized fields from request bodies.
   - Code example:
   ```javascript
   // ✅ SECURE - Property filtering
   function sanitizeRegistrationInput(input) {
     const allowedFields = ['email', 'password', 'passwordRepeat'];
     const sanitized = {};
     
     allowedFields.forEach(field => {
       if (input[field]) {
         sanitized[field] = input[field];
       }
     });
     
     // Explicitly exclude role and other sensitive fields
     return sanitized;
   }
   
   app.post('/api/Users', (req, res) => {
     const sanitized = sanitizeRegistrationInput(req.body);
     const user = {
       ...sanitized,
       role: 'customer' // Always set server-side
     };
     db.users.create(user);
   });
   ```

5. **Implement Proper Authorization Checks**
   - Implementation: Add middleware to verify that users cannot escalate their own privileges.
   - Code example:
   ```javascript
   // ✅ SECURE - Authorization middleware
   function requireAdmin(req, res, next) {
     if (req.user.role !== 'admin') {
       return res.status(403).json({ error: 'Admin access required' });
     }
     next();
   }
   
   // Only admins can create admin users
   app.post('/api/Users/admin', requireAdmin, (req, res) => {
     const user = {
       email: req.body.email,
       password: hashPassword(req.body.password),
       role: 'admin' // Only set here, with admin authorization
     };
     db.users.create(user);
   });
   ```

6. **Use Database Constraints**
   - Implementation: Set default values and constraints at the database level.
   - Code example:
   ```sql
   -- ✅ SECURE - Database constraints
   CREATE TABLE users (
     id INT PRIMARY KEY AUTO_INCREMENT,
     email VARCHAR(255) UNIQUE NOT NULL,
     password_hash VARCHAR(255) NOT NULL,
     role VARCHAR(50) DEFAULT 'customer' NOT NULL,
     CHECK (role IN ('customer', 'admin'))
   );
   ```

#### Security Best Practices:
- Never accept role or privilege parameters from client requests
- Always assign roles and permissions on the server side
- Implement proper RBAC (Role-Based Access Control) systems
- Validate and sanitize all user input
- Use whitelisting for allowed fields in requests
- Implement authorization checks for all privileged operations
- Set default roles at the database level
- Log all role assignments and privilege changes
- Regularly audit user roles and permissions
- Implement principle of least privilege
- Use separate endpoints for admin user creation
- Require existing admin authorization to create admin accounts

---

## Tools Used

| Tool | Purpose | Version/Link |
|------|---------|--------------|
| Burp Suite | HTTP request interception and manipulation | [Burp Suite Community](https://portswigger.net/burp/communitydownload) |
| Burp Interceptor | Capturing and modifying requests in real-time | Included in Burp Suite |
| Browser Developer Tools | Inspect API responses and user objects | Built-in |
| OWASP Juice Shop | Target application for testing | [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) |

---

## References & Further Reading

1. [OWASP Top 10 - A01:2021 Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
2. [OWASP - Access Control Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Access_Control_Cheat_Sheet.html)
3. [OWASP - Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
4. [CWE-269: Improper Privilege Management](https://cwe.mitre.org/data/definitions/269.html)
5. [PortSwigger - Privilege Escalation](https://portswigger.net/web-security/access-control)

---

## Notes & Reflections

### What I Learned
This challenge demonstrated how broken access control can allow attackers to escalate privileges by manipulating client-side input. The vulnerability occurred because the application trusted the client to provide the role parameter during registration, without validating that users shouldn't be able to assign themselves administrative roles. The experience highlighted the critical importance of never trusting client-side input for authorization decisions and always assigning roles and permissions on the server side.

### Challenges Faced
The main challenge was discovering that the role attribute existed and could be manipulated. Examining API responses and user objects helped identify the role field. Once identified, using Burp Suite to intercept and modify the registration request was straightforward. The key was understanding that the application would accept the role parameter without proper validation.

### Additional Observations
- Broken access control is the #1 vulnerability in the OWASP Top 10
- Client-side input should never be trusted for authorization decisions
- Role assignments must always be performed server-side
- Proper RBAC systems are essential for secure applications
- Input validation should explicitly whitelist allowed fields
- Database-level constraints can provide an additional security layer
- The challenge emphasizes the need for defense in depth - multiple security controls

---

**⚠️ Disclaimer:** This documentation is created solely for educational purposes as part of a DevSecOps training program. All activities were performed in a controlled, legal environment (OWASP Juice Shop). Never attempt these techniques on systems you don't own or have explicit permission to test.

---

**Date Completed:** 2025-11-16  
**Author:** Uwe Wohlleber

