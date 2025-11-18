# Meta Geo Stalking - ⭐⭐⭐

## Overview

**Category:** Information Disclosure / Metadata Exposure  
**Difficulty:** ⭐⭐⭐ (3/6)  
**OWASP Top 10:** A01:2021 – Broken Access Control / A04:2021 – Insecure Design

### Brief Description
This challenge demonstrates how metadata embedded in images (EXIF data) can reveal sensitive location information. By downloading images from the application's photo wall and extracting GPS coordinates using ExifTool, we can identify the physical location where photos were taken. This location information can then be used to discover user passwords or other sensitive information, demonstrating the privacy risks of sharing images with embedded metadata.

---

## Security Vulnerability Explained

### What is Metadata Exposure?
Metadata exposure occurs when applications or users share files (especially images) that contain embedded metadata such as EXIF (Exchangeable Image File Format) data. This metadata can include GPS coordinates, camera settings, timestamps, device information, and even software used to edit the image. When images are uploaded to web applications without stripping this metadata, attackers can extract sensitive information that reveals user locations, habits, or other private details. This information can then be used for stalking, social engineering, or password discovery.

### Dangers and Risks
Metadata exposure in images poses significant privacy and security risks:

- **Location Privacy:** GPS coordinates embedded in images can reveal where users live, work, or frequently visit, enabling physical stalking or targeted attacks.
- **Password Discovery:** Users often create passwords based on personal information, including favorite locations. Extracted location data can be used in password guessing attacks.
- **Social Engineering:** Location data combined with timestamps can reveal user routines, travel patterns, and social connections, enabling sophisticated social engineering attacks.
- **Physical Security:** Revealing exact locations can compromise physical security, especially for high-profile individuals or sensitive locations.
- **Business Consequences:**
  - Privacy violations and regulatory fines (GDPR, CCPA)
  - Loss of user trust and reputation damage
  - Legal liability for failing to protect user privacy
  - Potential physical security incidents
- **Real-world Examples:**
  - *2012:* A photo posted by a soldier revealed the location of a secret military base through embedded GPS coordinates
  - *Multiple incidents:* Celebrities and public figures have had their home addresses exposed through photo metadata
  - *Social media platforms:* Many users unknowingly share location data through photo uploads

### Why This Matters
In the age of social media and photo sharing, users often don't realize that their images contain sensitive metadata. Applications that accept image uploads have a responsibility to strip or sanitize this metadata to protect user privacy. Understanding how metadata can be extracted and used maliciously helps developers implement proper security measures and educates users about the risks of sharing unprocessed images.

---

## Challenge Documentation

### Challenge Description
The "Meta Geo Stalking" challenge requires extracting GPS coordinates from images uploaded to the application's photo wall and using this location information to discover a user's password. By downloading all images from the photo wall, using ExifTool to extract embedded GPS coordinates, and identifying the location (such as "Daniel Boone National Forest" in Kentucky), we can discover that a user has used this location as their password. This demonstrates how metadata exposure can lead to account compromise and privacy violations.

### Prerequisites
- [x] OWASP Juice Shop application running locally or remotely
- [x] ExifTool installed on the system
- [x] Access to the photo wall feature
- [x] Basic understanding of EXIF data and GPS coordinates
- [x] Knowledge of how to extract and process metadata from images
- [x] Optional: AI chat tool (like ChatGPT) for coordinate formatting and location identification

---

## Exploitation Steps

### Step 1: Reconnaissance
**Goal:** Identify the photo wall feature and understand what images are available.

**Actions:**
1. Navigate to the Juice Shop application
2. Locate the photo wall feature (where users can upload and view images)
3. Identify all images displayed on the photo wall
4. Note any images that might contain location information (e.g., hiking photos, travel photos)

**Observations:**
- The photo wall displays multiple user-uploaded images
- Some images appear to be from outdoor activities (e.g., "Love going Hiking")
- Images may contain embedded metadata including GPS coordinates
- Users may have used location-based passwords

---

### Step 2: Download Images
**Goal:** Download all images from the photo wall for metadata analysis.

**Actions:**
1. Download all images from the photo wall
2. Save images to a local directory for analysis
3. Organize images for systematic metadata extraction

**Evidence:**
- All images from the photo wall are now available locally
- Images are ready for metadata extraction using ExifTool

---

### Step 3: Extract GPS Metadata
**Goal:** Extract GPS coordinates from the downloaded images using ExifTool.

**Actions:**
1. Install ExifTool if not already available:
   ```bash
   # macOS
   brew install exiftool
   
   # Linux
   sudo apt-get install libimage-exiftool-perl
   
   # Or download from: https://exiftool.org/
   ```

2. Extract GPS coordinates from all downloaded images:
   ```bash
   exiftool -GPS:all -n *.jpg
   ```
   Or for more detailed output:
   ```bash
   exiftool -GPSLatitude -GPSLongitude -GPSLatitudeRef -GPSLongitudeRef -n *.jpg
   ```

3. Identify images that contain GPS coordinates
4. Note the GPS coordinates for further processing

**Evidence:**
- GPS coordinates are extracted from one or more images
- Coordinates are in decimal or degree format
- The coordinates point to a specific geographic location

---

### Step 4: Identify Location
**Goal:** Convert GPS coordinates to a human-readable location name.

**Actions:**
1. Use the extracted GPS coordinates to identify the location
2. Options for location identification:
   - Use an AI chat tool (like ChatGPT) to quickly convert coordinates to location names
   - Use online tools like Google Maps, GPS coordinates converter, or reverse geocoding services
   - Manually look up coordinates on a map

3. Example using ChatGPT or similar:
   ```
   "Convert these GPS coordinates to a location name: [latitude], [longitude]"
   ```

4. Identify the location (e.g., "Daniel Boone National Forest, Kentucky")

**Evidence:**
- GPS coordinates are successfully converted to a location name
- The location is identified (e.g., a national forest, park, or landmark)
- Multiple location suggestions may be provided, with the correct one being the user's password

---

### Step 5: Find Target User
**Goal:** Identify which user's password is based on the discovered location.

**Actions:**
1. Check the admin panel or user administration section
2. Review user lists and profiles
3. Check user reviews or other user-generated content
4. Look for users who might be associated with the discovered location
5. Note: The target user (e.g., "John") may not be in the admin user list but can be found through reviews or other sections

**Evidence:**
- Target user is identified (e.g., "John")
- User may be found through reviews or other user-generated content
- User profile or activity suggests connection to the discovered location

---

### Step 6: Exploitation
**Goal:** Use the discovered location as a password to authenticate as the target user.

**Payload/Technique:**
Use the location name (or variations of it) as the password for the identified user account.

**Execution:**
1. Navigate to the login page
2. Enter the target user's email/username
3. Try the location name as the password:
   - Full location name: "Daniel Boone National Forest"
   - Variations: "danielboonenationalforest", "DanielBooneNationalForest", etc.
4. If the first attempt fails, try common variations:
   - With/without spaces
   - Different capitalization
   - Abbreviated versions

**Result:**
- ✅ Successfully authenticated using the location-based password
- Challenge solved notification appears
- Access to the user account is granted
- This demonstrates how metadata exposure can lead to account compromise

---

## Video Demonstration

### 🎥 Loom Video
**Link:** [Loom Video](https://www.loom.com/share/b915558227f54eba8819e6928da5583d)

**Video Contents:**
- Introduction to Meta Geo Stalking challenge (0:00 - 0:02)
  - Overview of the challenge and ExifTool
- Downloading images from photo wall (0:02 - 0:17)
  - Accessing the photo wall feature
  - Downloading all images (including "Love going Hiking" photo)
- Extracting GPS data with ExifTool (0:17 - 0:45)
  - Using ExifTool to extract GPS coordinates
  - Identifying images with embedded location data
- Using AI chat for coordinate conversion (0:45 - 1:12)
  - Using ChatGPT or similar tool to convert coordinates to location names
  - Getting location suggestions quickly
- Finding the target user (1:12 - 1:30)
  - Searching through admin panel and user lists
  - Finding user "John" through reviews (not in admin list)
  - Using the registry or other sections to locate the user
- Identifying the location (1:30 - 2:18)
  - Discovering "Daniel Boone National Forest" in Kentucky
  - Confirming the location matches the GPS coordinates
- Using location as password (2:18 - 2:30)
  - Attempting login with location-based password
  - Successful authentication
  - Challenge completion

---

## Mitigation & Prevention

### How to Fix This Vulnerability

#### Developer Recommendations:

1. **Strip Metadata from Uploaded Images**
   - Implementation: Remove all EXIF and metadata from images before storing them on the server.
   - Code example:
   ```javascript
   // ✅ SECURE - Strip metadata using sharp library
   const sharp = require('sharp');
   
   async function stripMetadata(imageBuffer) {
     const stripped = await sharp(imageBuffer)
       .rotate() // Auto-rotate based on EXIF
       .removeAlpha() // Remove alpha channel if not needed
       .jpeg({ quality: 85 }) // Re-encode to strip metadata
       .toBuffer();
     
     return stripped;
   }
   
   app.post('/api/upload', upload.single('image'), async (req, res) => {
     const imageBuffer = fs.readFileSync(req.file.path);
     const cleanedImage = await stripMetadata(imageBuffer);
     
     // Save cleaned image
     fs.writeFileSync(req.file.path, cleanedImage);
     
     // Process image...
   });
   ```

2. **Use ExifTool Server-Side**
   - Implementation: Use ExifTool on the server to strip all metadata from uploaded images.
   - Code example:
   ```javascript
   // ✅ SECURE - Strip metadata with ExifTool
   const { exec } = require('child_process');
   const util = require('util');
   const execPromise = util.promisify(exec);
   
   async function stripMetadataExifTool(filePath) {
     try {
       // Remove all metadata
       await execPromise(`exiftool -all= -overwrite_original "${filePath}"`);
       return true;
     } catch (error) {
       console.error('Error stripping metadata:', error);
       return false;
     }
   }
   ```

3. **Validate and Sanitize Image Files**
   - Implementation: Re-encode images to remove metadata and ensure they're valid image files.
   - Code example:
   ```javascript
   // ✅ SECURE - Re-encode image to strip metadata
   const Jimp = require('jimp');
   
   async function sanitizeImage(inputPath, outputPath) {
     const image = await Jimp.read(inputPath);
     
     // Re-encode image (this strips metadata)
     await image
       .quality(85)
       .write(outputPath);
     
     return outputPath;
   }
   ```

4. **Implement Privacy Controls**
   - Implementation: Allow users to choose whether to include location data in their uploads.
   - Code example:
   ```javascript
   // ✅ SECURE - User-controlled privacy
   app.post('/api/upload', upload.single('image'), async (req, res) => {
     const includeLocation = req.body.includeLocation === 'true';
     
     if (!includeLocation) {
       // Strip GPS data specifically
       await execPromise(`exiftool -GPS:all= -overwrite_original "${req.file.path}"`);
     }
     
     // Process image...
   });
   ```

5. **Educate Users About Metadata**
   - Implementation: Display warnings about metadata in images and provide options to remove it.
   - Code example:
   ```javascript
   // ✅ SECURE - User education
   // In frontend:
   <div class="metadata-warning">
     <p>⚠️ Your image may contain location data. We will remove this for your privacy.</p>
     <label>
       <input type="checkbox" name="stripMetadata" checked>
       Remove location and metadata from this image
     </label>
   </div>
   ```

6. **Implement Content Security Policy**
   - Implementation: Use CSP headers to prevent metadata extraction through client-side scripts.
   - Code example:
   ```javascript
   // ✅ SECURE - CSP headers
   app.use((req, res, next) => {
     res.setHeader('Content-Security-Policy', "default-src 'self'");
     next();
   });
   ```

#### Security Best Practices:
- Always strip metadata from uploaded images on the server side
- Re-encode images to remove all embedded data
- Use ExifTool or similar tools to sanitize image files
- Implement user privacy controls for location sharing
- Educate users about metadata risks
- Validate and sanitize all uploaded files
- Implement proper access controls for user data
- Log metadata stripping operations for audit purposes
- Regularly audit image upload functionality
- Consider using image processing libraries that automatically strip metadata

---

## Tools Used

| Tool | Purpose | Version/Link |
|------|---------|--------------|
| ExifTool | Extract and manipulate image metadata | [ExifTool](https://exiftool.org/) |
| Browser Developer Tools | Download images from photo wall | Built-in |
| ChatGPT / AI Chat | Convert GPS coordinates to location names | [ChatGPT](https://chat.openai.com/) or similar |
| Google Maps / Geocoding Services | Alternative method for coordinate conversion | [Google Maps](https://www.google.com/maps) |
| OWASP Juice Shop | Target application for testing | [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) |

---

## References & Further Reading

1. [OWASP - Information Exposure](https://owasp.org/www-community/vulnerabilities/Information_exposure)
2. [ExifTool Documentation](https://exiftool.org/)
3. [OWASP Top 10 - A01:2021 Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
4. [OWASP Top 10 - A04:2021 Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/)
5. [Privacy Risks of Photo Metadata](https://www.eff.org/deeplinks/2012/04/picture-worth-thousand-words-including-where-you-live)
6. [GDPR and Metadata](https://gdpr.eu/data-protection-by-design-and-by-default/)

---

## Notes & Reflections

### What I Learned
This challenge demonstrated how easily sensitive location information can be extracted from images through embedded metadata. The use of ExifTool made it straightforward to extract GPS coordinates, and AI tools like ChatGPT made it quick to convert coordinates to location names. The experience highlighted the importance of stripping metadata from uploaded images and the privacy risks of sharing unprocessed photos. Understanding how metadata can be used maliciously helps developers implement proper security measures.

### Challenges Faced
The main challenge was efficiently converting GPS coordinates to location names. Using an AI chat tool proved to be much faster than manually formatting and looking up coordinates. Additionally, finding the target user required checking multiple sections of the application, as the user wasn't immediately visible in the admin user list but could be found through reviews or other user-generated content sections.

### Additional Observations
- Metadata in images can reveal much more than just location (timestamps, device info, etc.)
- Users often create passwords based on personal information, including favorite locations
- The challenge emphasizes the need for both technical controls (metadata stripping) and user education
- AI tools can significantly speed up security research and exploitation tasks
- Privacy by design should include automatic metadata removal for all user uploads

---

**⚠️ Disclaimer:** This documentation is created solely for educational purposes as part of a DevSecOps training program. All activities were performed in a controlled, legal environment (OWASP Juice Shop). Never attempt these techniques on systems you don't own or have explicit permission to test.

---

**Date Completed:** 2025-11-16  
**Author:** Uwe Wohlleber

