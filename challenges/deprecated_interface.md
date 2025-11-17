# Deprecated Interface - ⭐⭐

## Overview

**Category:** Insecure File Upload / Client-Side Validation Bypass  
**Difficulty:** ⭐⭐ (2/6)  
**OWASP Top 10:** A03:2021 – Injection / A05:2021 – Security Misconfiguration

### Brief Description
This challenge demonstrates how client-side file type validation can be bypassed by examining the JavaScript code and manipulating the file upload mechanism. The application appears to restrict file uploads to PDF files only, but by inspecting the JavaScript code handling the upload, we discover that additional file types are actually accepted. By using the browser's file picker to select "All Files" and uploading a non-PDF file (such as XML), we can successfully bypass the intended restrictions and upload unauthorized file types.

---

## Security Vulnerability Explained

### What is Insecure File Upload?
Insecure file upload vulnerabilities occur when applications fail to properly validate file types, sizes, or content on the server side. Many applications rely solely on client-side validation (JavaScript in the browser) to restrict file uploads, which can be easily bypassed. Attackers can examine the client-side code, manipulate file upload requests, or use browser features to upload files that should be restricted. This can lead to various security issues including malware uploads, server-side code execution, or data exfiltration.

### Dangers and Risks
Insecure file upload vulnerabilities pose significant security risks:

- **Malware Distribution:** Attackers can upload malicious files (viruses, trojans, ransomware) that can infect other users or the server itself.
- **Server-Side Code Execution:** Uploading executable scripts (PHP, JSP, etc.) can allow attackers to execute arbitrary code on the server, potentially gaining full system control.
- **Data Exfiltration:** Malicious files can be designed to extract sensitive data from the server or database.
- **Denial of Service:** Large files or resource-intensive file types can consume server resources and cause service disruption.
- **Business Consequences:**
  - Complete system compromise and data breaches
  - Regulatory fines for security failures
  - Loss of customer trust and reputation damage
  - Legal liability for security incidents
- **Real-world Examples:**
  - *2017 Equifax breach:* Attackers exploited file upload vulnerabilities to gain initial access
  - *Multiple CMS platforms:* File upload vulnerabilities have been exploited to deface websites and steal data

### Why This Matters
File upload functionality is a common feature in web applications, but it's also one of the most dangerous if not properly secured. Client-side validation provides a false sense of security and can always be bypassed. Server-side validation, file type verification, content scanning, and proper file storage are essential to prevent attackers from uploading malicious files. Understanding how to bypass client-side restrictions helps developers recognize the importance of implementing comprehensive server-side security controls.

---

## Challenge Documentation

### Challenge Description
The "Deprecated Interface" challenge requires bypassing client-side file type validation to upload a non-PDF file. The application's upload form appears to restrict file uploads to PDF files only, but by examining the JavaScript code that handles the upload, we discover that the validation logic actually accepts additional file types. By using the browser's file picker to select "All Files" instead of the filtered view, we can select and upload files of other types (such as XML), successfully bypassing the intended restrictions and demonstrating the insecurity of relying solely on client-side validation.

### Prerequisites
- [x] OWASP Juice Shop application running locally or remotely
- [x] Browser with Developer Tools (Chrome recommended)
- [x] Burp Suite (optional, for request inspection)
- [x] Basic understanding of JavaScript and browser developer tools
- [x] Knowledge of file upload mechanisms and validation

---

## Exploitation Steps

### Step 1: Reconnaissance
**Goal:** Identify the file upload interface and understand its apparent restrictions.

**Actions:**
1. Navigate to the Juice Shop application
2. Locate the file upload interface (likely in the "Complaint" or similar section)
3. Observe that the upload form appears to restrict file types to PDF only
4. Note the file picker dialog shows only PDF files when opened normally

**Observations:**
- The upload form displays a file input field
- The file picker dialog appears to filter for PDF files only
- The interface suggests only PDF uploads are allowed
- Client-side validation appears to be in place

---

### Step 2: Vulnerability Identification
**Goal:** Examine the JavaScript code to discover the actual file type validation logic.

**Actions:**
1. Open the browser's Developer Tools (F12 or right-click → Inspect)
2. Navigate to the Console tab
3. Examine the JavaScript code that handles the file upload
4. Search for file type validation logic in the JavaScript
5. Look for the `accept` attribute or file type checking code

**Evidence:**
- The JavaScript code reveals the file upload handling logic
- Examination shows that while PDF is the primary allowed type, additional file types are actually accepted
- The code may show three or more file types that are permitted
- The client-side validation is more permissive than the UI suggests

---

### Step 3: Exploitation
**Goal:** Bypass the file type restriction and successfully upload a non-PDF file.

**Payload/Technique:**
Upload a file of an unauthorized type (e.g., XML, XSS payload, or other file type) by:
1. Using the browser's file picker dialog
2. Changing the file type filter to "All Files" instead of "PDF Files"
3. Selecting a non-PDF file (e.g., an XML file)
4. Submitting the upload

**Execution:**
1. Navigate to the file upload interface in the Juice Shop application
2. Click on the file upload button/field
3. In the file picker dialog that opens:
   - Look for the file type filter dropdown (usually at the bottom)
   - Change the filter from "PDF Files" or "PDF Documents" to "All Files" or "*.*"
4. Navigate to and select a non-PDF file (e.g., an XML file, XSS payload file, or other file type)
5. Click "Open" or "Select" to confirm the file selection
6. Submit the upload form

**Alternative Method (if file picker doesn't allow "All Files"):**
1. Use Burp Suite to intercept the upload request
2. Modify the `Content-Type` header or file extension in the request
3. Forward the modified request

**Result:**
- ✅ The non-PDF file is successfully uploaded
- Success message appears confirming the upload
- The challenge is marked as completed
- This demonstrates that client-side validation can be easily bypassed

---

## Video Demonstration

### 🎥 Loom Video
**Link:** [Loom Video](https://www.loom.com/share/22e248028a6c424a977ded94f15a6b14)

**Video Contents:**
- Introduction to the Deprecated Interface challenge (0:00 - 0:02)
  - Overview of the file upload interface
  - Apparent PDF-only restriction
- Using Chrome/Burp Suite browser (0:02 - 0:24)
  - Setting up the testing environment
  - Navigating to the upload interface
- Examining JavaScript code (0:24 - 0:35)
  - Opening browser Developer Tools
  - Accessing the JavaScript console
- Discovering file type validation (0:35 - 1:11)
  - Searching for PDF-related code
  - Finding that three additional file types are accepted
  - Understanding the JavaScript upload handling logic
- Preparing the exploit file (1:11 - 1:26)
  - Creating or selecting an XML/XSS file
  - Understanding the file type to upload
- Bypassing the file picker restriction (1:26 - 1:50)
  - Opening the file picker dialog
  - Changing the file type filter to "All Files"
  - Selecting the non-PDF file
- Successful upload (1:50 - 2:00)
  - Submitting the upload
  - Receiving success confirmation
  - Challenge completion

---

## Mitigation & Prevention

### How to Fix This Vulnerability

#### Developer Recommendations:

1. **Implement Server-Side File Type Validation**
   - Implementation: Always validate file types on the server side using the file's actual content (magic bytes/MIME type), not just the file extension or Content-Type header.
   - Code example:
   ```javascript
   // ❌ VULNERABLE - Client-side only validation
   <input type="file" accept=".pdf" />
   
   // ✅ SECURE - Server-side validation
   const fileType = require('file-type');
   const fs = require('fs');
   
   app.post('/api/upload', upload.single('file'), async (req, res) => {
     const file = req.file;
     
     // Read file content to determine actual type
     const buffer = fs.readFileSync(file.path);
     const type = await fileType.fromBuffer(buffer);
     
     // Whitelist allowed MIME types
     const allowedTypes = ['application/pdf'];
     if (!allowedTypes.includes(type.mime)) {
       fs.unlinkSync(file.path); // Delete uploaded file
       return res.status(400).json({ error: 'Invalid file type' });
     }
     
     // Process file...
   });
   ```

2. **Validate File Extensions and Content**
   - Implementation: Check both file extension and actual file content to ensure they match. Never trust the Content-Type header alone.
   - Code example:
   ```javascript
   // ✅ SECURE - Validate extension and content
   const path = require('path');
   const fileType = require('file-type');
   
   function validateFile(file) {
     const ext = path.extname(file.originalname).toLowerCase();
     const allowedExtensions = ['.pdf'];
     
     // Check extension
     if (!allowedExtensions.includes(ext)) {
       throw new Error('Invalid file extension');
     }
     
     // Check actual file content
     const type = fileType.fromBuffer(file.buffer);
     if (type.mime !== 'application/pdf') {
       throw new Error('File content does not match extension');
     }
     
     return true;
   }
   ```

3. **Implement File Size Limits**
   - Implementation: Enforce maximum file size limits to prevent DoS attacks and resource exhaustion.
   - Code example:
   ```javascript
   // ✅ SECURE - File size validation
   const multer = require('multer');
   const upload = multer({
     limits: {
       fileSize: 5 * 1024 * 1024 // 5MB limit
     },
     fileFilter: (req, file, cb) => {
       // Validate file type...
       cb(null, true);
     }
   });
   ```

4. **Sanitize File Names**
   - Implementation: Sanitize uploaded file names to prevent path traversal and other attacks. Generate unique names for stored files.
   - Code example:
   ```javascript
   // ✅ SECURE - Sanitize and rename files
   const path = require('path');
   const crypto = require('crypto');
   
   function sanitizeFileName(originalName) {
     // Remove path components
     const basename = path.basename(originalName);
     
     // Remove special characters
     const sanitized = basename.replace(/[^a-zA-Z0-9.-]/g, '_');
     
     // Generate unique name
     const hash = crypto.randomBytes(16).toString('hex');
     const ext = path.extname(sanitized);
     
     return `${hash}${ext}`;
   }
   ```

5. **Store Files Outside Web Root**
   - Implementation: Store uploaded files outside the web-accessible directory to prevent direct execution or access.
   - Code example:
   ```javascript
   // ✅ SECURE - Store files outside web root
   const uploadDir = path.join(__dirname, '../uploads'); // Outside public directory
   
   const storage = multer.diskStorage({
     destination: (req, file, cb) => {
       cb(null, uploadDir);
     },
     filename: (req, file, cb) => {
       cb(null, sanitizeFileName(file.originalname));
     }
   });
   ```

6. **Scan Files for Malware**
   - Implementation: Use antivirus scanning or content analysis to detect malicious files before processing.
   - Code example:
   ```javascript
   // ✅ SECURE - Malware scanning
   const ClamScan = require('clamscan');
   
   async function scanFile(filePath) {
     const clamscan = await new ClamScan().init();
     const { isInfected, viruses } = await clamscan.isInfected(filePath);
     
     if (isInfected) {
       throw new Error(`File is infected: ${viruses.join(', ')}`);
     }
   }
   ```

7. **Use Content Security Policy**
   - Implementation: Implement Content Security Policy headers to prevent execution of uploaded files.
   - Code example:
   ```javascript
   // ✅ SECURE - CSP headers
   app.use((req, res, next) => {
     res.setHeader('Content-Security-Policy', "default-src 'self'");
     next();
   });
   ```

#### Security Best Practices:
- Always validate file types on the server side using actual file content
- Never trust client-side validation alone
- Use whitelisting for allowed file types (not blacklisting)
- Validate both file extension and MIME type/content
- Implement file size limits
- Sanitize and rename uploaded files
- Store files outside the web root directory
- Scan files for malware before processing
- Use Content Security Policy headers
- Implement proper access controls for uploaded files
- Log all file upload attempts for security monitoring
- Regularly audit file upload functionality

---

## Tools Used

| Tool | Purpose | Version/Link |
|------|---------|--------------|
| Browser Developer Tools | Inspect JavaScript code and console | Built-in (Chrome/Firefox) |
| Chrome Browser | Testing browser with DevTools | [Chrome](https://www.google.com/chrome/) |
| Burp Suite | Optional: Request inspection and manipulation | [Burp Suite Community](https://portswigger.net/burp/communitydownload) |
| OWASP Juice Shop | Target application for testing | [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) |

---

## References & Further Reading

1. [OWASP - Unrestricted File Upload](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
2. [OWASP - File Upload Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html)
3. [OWASP Top 10 - A03:2021 Injection](https://owasp.org/Top10/A03_2021-Injection/)
4. [OWASP Top 10 - A05:2021 Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)
5. [PortSwigger - File Upload Vulnerabilities](https://portswigger.net/web-security/file-upload)

---

## Notes & Reflections

### What I Learned
This challenge demonstrated how easily client-side file type validation can be bypassed. By simply examining the JavaScript code and using the browser's file picker to select "All Files", I was able to upload file types that should have been restricted. The experience highlighted the critical importance of implementing server-side validation and not relying on client-side restrictions for security. Understanding how to bypass these restrictions helps developers recognize the need for comprehensive server-side security controls.

### Challenges Faced
The main challenge was discovering that the JavaScript code actually accepted more file types than the UI suggested. Examining the JavaScript console and understanding the file upload handling code was crucial. Additionally, finding the "All Files" option in the file picker dialog required some exploration, as it's not always immediately visible depending on the browser and operating system.

### Additional Observations
- Client-side validation provides a false sense of security and can always be bypassed
- The file picker's file type filter is a UI convenience, not a security control
- JavaScript code can reveal hidden functionality or less restrictive validation
- Server-side validation is essential for all file upload functionality
- The challenge emphasizes the need for defense in depth - multiple layers of security controls

---

**⚠️ Disclaimer:** This documentation is created solely for educational purposes as part of a DevSecOps training program. All activities were performed in a controlled, legal environment (OWASP Juice Shop). Never attempt these techniques on systems you don't own or have explicit permission to test.

---

**Date Completed:** 2025-11-16  
**Author:** Uwe Wohlleber

