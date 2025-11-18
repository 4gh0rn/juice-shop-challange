# Restore Progress - Backup Challenge - ⭐⭐⭐

## Overview

**Category:** [API Manipulation / Insecure Direct Object Reference (IDOR)]  
**Difficulty:** [3 stars]  
**OWASP Top 10:** A01:2021 – Broken Access Control / A04:2021 – Insecure Design

### Brief Description
This bonus challenge involves restoring the challenge progress in Juice Shop by exploiting an insecure backup/restore mechanism. The application allows users to backup and restore their challenge progress, but the implementation contains a vulnerability that allows manipulation of the restore process. By intercepting and modifying the restore request using Burp Suite, we can send a PUT request with a continue code that maps to the progress, effectively restoring all completed challenges without needing the actual backup file. This demonstrates how insecure API design and lack of proper access controls can allow unauthorized manipulation of user data.

---

## Security Vulnerability Explained

### What is Insecure Direct Object Reference (IDOR)?
Insecure Direct Object Reference (IDOR) is a type of access control vulnerability that occurs when an application provides direct access to objects based on user-supplied input without proper authorization checks. In this case, the application allows users to reference their progress data directly through a continue code or identifier without verifying that the user has legitimate access to that specific progress state. Attackers can manipulate these references to access or modify data belonging to other users, or in this case, restore progress states they shouldn't have access to.

### Dangers and Risks
IDOR and insecure API design expose critical security risks:

- **Unauthorized Data Access:** Attackers can access or restore data belonging to other users by manipulating object references, leading to privacy violations and data breaches.
- **Data Manipulation:** Users can modify or restore states they shouldn't have access to, potentially bypassing business logic or security controls.
- **Privilege Escalation:** By manipulating object references, attackers might gain access to administrative functions or sensitive data beyond their authorized level.
- **Business Logic Bypass:** Insecure restore mechanisms can allow users to bypass intended restrictions, such as restoring progress without proper verification.
- **Real-world Examples:**
  - *Facebook (2018):* IDOR vulnerability allowed access to private photos of users through manipulated API requests.
  - *Multiple SaaS platforms:* IDOR vulnerabilities have allowed attackers to access other users' data by changing user IDs in API requests.

### Why This Matters
Insecure Direct Object References and poorly designed API endpoints are common vulnerabilities in modern web applications. When applications expose internal object references (like IDs, codes, or tokens) without proper authorization checks, attackers can easily manipulate these references to access unauthorized data. This vulnerability is particularly dangerous because it's often easy to exploit and can lead to significant data breaches. Understanding how to identify and exploit these vulnerabilities helps developers implement proper access controls and authorization checks.

---

## Challenge Documentation

### Challenge Description
The "Restore Progress" bonus challenge requires exploiting an insecure backup/restore mechanism in Juice Shop. The application allows users to backup their challenge progress, which generates a continue code. However, the restore functionality accepts this continue code via a PUT request without proper validation or authorization checks. By intercepting the restore request in Burp Suite and modifying it, we can send a PUT request with a continue code that maps directly to the progress state, effectively restoring all completed challenges. This bypasses the intended backup file requirement and demonstrates how insecure API design can allow unauthorized state manipulation.

### Prerequisites
- [x] OWASP Juice Shop application running locally or remotely
- [x] Burp Suite Community/Professional Edition installed
- [x] Browser configured to use Burp Suite as proxy
- [x] Basic understanding of HTTP methods (PUT requests)
- [x] Knowledge of API manipulation and request interception
- [x] Understanding of how backup/restore mechanisms work

---

## Exploitation Steps

### Step 1: Reconnaissance
**Goal:** Identify the backup/restore functionality and understand how it works.

**Actions:**
1. Navigate to the Juice Shop application
2. Look for backup/restore functionality in the user interface
3. Open Burp Suite and configure the browser proxy settings
4. Enable Burp Interceptor to capture HTTP requests
5. Attempt to use the backup functionality to generate a continue code
6. Observe the backup request structure and response

**Observations:**
- The application has a backup feature that generates a continue code
- The continue code maps to the user's challenge progress
- The restore functionality likely uses a PUT request to restore progress
- The continue code is sent in the request body or as a parameter

---

### Step 2: Vulnerability Identification
**Goal:** Identify the insecure restore endpoint and understand how the continue code works.

**Actions:**
1. Capture the restore request using Burp Suite Interceptor
2. Analyze the request structure:
   - HTTP method (likely PUT)
   - Endpoint URL
   - Request body containing the continue code
   - Headers and authentication tokens
3. Identify that the continue code directly maps to progress without proper validation
4. Note that the restore can be triggered without the actual backup file

**Evidence:**
- The restore endpoint accepts a PUT request with a continue code
- The continue code is sent in the request body (likely as JSON)
- No additional validation or file upload is required
- The continue code directly maps to challenge progress state

---

### Step 3: Exploitation
**Goal:** Restore challenge progress by sending a manipulated PUT request with a continue code.

**Payload/Technique:**
```http
PUT /rest/user/data/restore HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Authorization: Bearer [token]

{
  "continueCode": "[continue-code-here]"
}
```

**Execution:**
1. Open Burp Suite Repeater
2. Find or capture a restore request (either from previous backup or by analyzing the application)
3. Copy the restore request into Burp Suite Repeater
4. Modify the request to include a continue code that maps to the desired progress
5. Ensure the PUT request includes:
   - Correct endpoint (e.g., `/rest/user/data/restore`)
   - Proper headers (Content-Type: application/json, Authorization)
   - Continue code in the request body
6. Send the request using Burp Suite Repeater
7. Observe the response confirming progress restoration

**Alternative Method (Using Interceptor):**
1. Enable Burp Suite Interceptor
2. Navigate to the backup/restore section in Juice Shop
3. Select "Backup" to generate a continue code (or use an existing one)
4. When the restore request is intercepted, modify it if needed
5. Forward the request to restore progress

**Result:**
- The PUT request successfully restores the challenge progress
- All challenges associated with the continue code are restored
- The application confirms the restoration without requiring the actual backup file
- The challenge is marked as completed
- This demonstrates that the restore mechanism doesn't properly validate authorization or require the backup file

---

## Video Demonstration

### 🎥 Loom Video
**Link:** [Loom Video](https://www.loom.com/share/ec71779cd20d4d97a90b16d2ae5868e6)

**Video Contents:**
- Introduction to the Restore Progress challenge (0:01 - 0:13)
- Overview of the approach and methodology (0:13 - 0:21)
- Using Burp Suite Repeater to prepare the request (0:21 - 0:33)
- Analyzing the request structure and continue code mapping (0:33 - 0:48)
- Demonstrating progress restoration without backup file (0:48 - 0:59)
- Using Burp Interceptor as alternative method (0:59 - 1:12)
- Sending PUT request and confirming restoration (1:12 - 1:30)
- Challenge completion confirmation (1:30 - end)

---

## Mitigation & Prevention

### How to Fix This Vulnerability

#### Developer Recommendations:
1. **Implement Proper Authorization Checks**
   - Implementation: Verify that the user requesting the restore operation is authorized to access the specific continue code or backup. Implement server-side validation that checks ownership of the backup/continue code.
   - Code example:
   ```javascript
   // ❌ VULNERABLE - No authorization check
   app.put('/rest/user/data/restore', (req, res) => {
     const continueCode = req.body.continueCode;
     const progress = getProgressByCode(continueCode);
     restoreProgress(req.user.id, progress);
   });
   
   // ✅ SECURE - Authorization check
   app.put('/rest/user/data/restore', authenticateUser, (req, res) => {
     const continueCode = req.body.continueCode;
     const progress = getProgressByCode(continueCode);
     
     // Verify ownership
     if (progress.userId !== req.user.id) {
       return res.status(403).json({ error: 'Unauthorized' });
     }
     
     restoreProgress(req.user.id, progress);
   });
   ```

2. **Require Backup File Upload**
   - Implementation: Instead of accepting just a continue code, require users to upload the actual backup file. Validate the file integrity and verify it matches the continue code before restoring.
   - Code example:
   ```javascript
   // ✅ SECURE - Require backup file
   app.put('/rest/user/data/restore', authenticateUser, upload.single('backup'), (req, res) => {
     const continueCode = req.body.continueCode;
     const backupFile = req.file;
     
     // Validate file integrity
     const fileHash = calculateHash(backupFile);
     const expectedHash = getHashForCode(continueCode);
     
     if (fileHash !== expectedHash) {
       return res.status(400).json({ error: 'Invalid backup file' });
     }
     
     // Verify ownership
     const progress = parseBackupFile(backupFile);
     if (progress.userId !== req.user.id) {
       return res.status(403).json({ error: 'Unauthorized' });
     }
     
     restoreProgress(req.user.id, progress);
   });
   ```

3. **Implement Time-Limited Continue Codes**
   - Implementation: Add expiration times to continue codes to limit the window of potential abuse. Invalidate codes after a reasonable time period.
   - Code example:
   ```javascript
   // ✅ SECURE - Time-limited codes
   const continueCode = {
     code: generateCode(),
     userId: user.id,
     expiresAt: new Date(Date.now() + 24 * 60 * 60 * 1000) // 24 hours
   };
   
   // During restore
   if (continueCode.expiresAt < new Date()) {
     return res.status(400).json({ error: 'Continue code expired' });
   }
   ```

4. **Use Cryptographically Secure Tokens**
   - Implementation: Use signed tokens (like JWT) for continue codes that include user ID and expiration, making them tamper-proof.
   - Code example:
   ```javascript
   // ✅ SECURE - Signed tokens
   const continueCode = jwt.sign(
     { userId: user.id, progressId: progress.id },
     SECRET_KEY,
     { expiresIn: '24h' }
   );
   
   // During restore
   try {
     const decoded = jwt.verify(continueCode, SECRET_KEY);
     if (decoded.userId !== req.user.id) {
       return res.status(403).json({ error: 'Unauthorized' });
     }
   } catch (error) {
     return res.status(400).json({ error: 'Invalid continue code' });
   }
   ```

5. **Implement Rate Limiting**
   - Implementation: Add rate limiting to prevent abuse of the restore endpoint, limiting how frequently users can restore progress.
   - Code example:
   ```javascript
   const rateLimit = require('express-rate-limit');
   const restoreLimiter = rateLimit({
     windowMs: 15 * 60 * 1000, // 15 minutes
     max: 5 // 5 restore attempts per 15 minutes
   });
   
   app.put('/rest/user/data/restore', restoreLimiter, ...);
   ```

#### Security Best Practices:
- Never trust client-supplied data - always validate and authorize on the server side
- Implement proper access controls for all API endpoints
- Use cryptographically secure tokens for sensitive operations
- Require additional verification (like file uploads) for critical operations
- Implement audit logging for restore operations to detect abuse
- Use time-limited tokens to reduce the window of potential exploitation
- Implement rate limiting on sensitive endpoints
- Regularly audit API endpoints for IDOR vulnerabilities
- Use indirect object references (mapping tables) instead of direct object references when possible

---

## Tools Used

| Tool | Purpose | Version/Link |
|------|---------|--------------|
| Burp Suite | HTTP request interception, modification, and replay | [Burp Suite Community](https://portswigger.net/burp/communitydownload) |
| Burp Repeater | Manipulating and resending HTTP requests | Included in Burp Suite |
| Burp Interceptor | Capturing and modifying requests in real-time | Included in Burp Suite |
| OWASP Juice Shop | Target application for testing | [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) |

---

## References & Further Reading

1. [OWASP Top 10 - A01:2021 Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
2. [OWASP Top 10 - A04:2021 Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/)
3. [OWASP - Insecure Direct Object Reference Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html)
4. [Burp Suite Documentation](https://portswigger.net/burp/documentation)
5. [OWASP Juice Shop Documentation](https://pwning.owasp-juice.shop/)
6. [API Security Best Practices - OWASP](https://owasp.org/www-project-api-security/)

---

## Notes & Reflections

### What I Learned
- Insecure Direct Object References (IDOR) can be exploited by manipulating API requests
- Backup/restore mechanisms need proper authorization checks to prevent unauthorized access
- Continue codes or tokens that directly map to user data are vulnerable if not properly secured
- Burp Suite Repeater is an excellent tool for testing API endpoints and manipulating requests
- PUT requests can be used to modify application state, making them a common target for exploitation
- Server-side validation is crucial - client-side checks can always be bypassed
- Understanding API structure and request/response patterns is essential for security testing

### Challenges Faced
- Identifying the correct endpoint and request structure for the restore operation
- Understanding how the continue code maps to challenge progress
- Determining the correct HTTP method (PUT) and request format
- Finding the continue code or understanding how to generate/obtain it
- Ensuring the request includes proper authentication headers

### Additional Observations
- The vulnerability demonstrates how seemingly simple features (backup/restore) can have serious security implications
- API endpoints that accept direct object references without authorization are common in web applications
- This type of vulnerability is often easy to exploit once identified, making proper prevention crucial
- The restore mechanism bypasses the intended backup file requirement, showing insecure design
- This is a bonus challenge, indicating it's a more advanced exploitation technique
- The continue code system is convenient for users but creates a security risk if not properly implemented

---

**⚠️ Disclaimer:** This documentation is created solely for educational purposes as part of a DevSecOps training program. All activities were performed in a controlled, legal environment (OWASP Juice Shop). Never attempt these techniques on systems you don't own or have explicit permission to test.

---

**Date Completed:** [2025-11-09]  
**Author:** [Uwe Wohlleber]

