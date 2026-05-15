# Advanced-Security-Assessment-of-the-web-Application
Basic Penetration Testing  Basic Penetration Testing and Create a Simple Checklist   

# OWASP Juice Shop: Security Assessment & Enhancement

![Cybersecurity](https://img.shields.io/badge/Main_Topic-Cybersecurity-blue.svg)
![Tools](https://img.shields.io/badge/Tools-Nmap%20|%20VS%20Code%20|%20Winston-green.svg)
![Status](https://img.shields.io/badge/Task-Week%203%20Complete-success.svg)

A comprehensive security audit and logging implementation performed on the **OWASP Juice Shop** (v20.0.0) deliberately insecure web application. This project demonstrates the identification of critical web vulnerabilities and the implementation of defensive logging mechanisms.

##  Project Objectives
- Perform Network Reconnaissance and Service Detection.
- Execute and document common web attacks (SQL Injection & XSS).
- Implement a robust server-side logging system using Node.js.
- Evaluate the application against a Security Best Practices Checklist.

---

##  Tech Stack & Tools
- **Environment:** Windows 11 / Node.js v22
- **Application:** OWASP Juice Shop
- **Security Tools:** - **Nmap/Zenmap:** Network scanning and footprinting.
  - **Browser DevTools:** Manual exploitation.
- **Libraries:** - **Winston:** Production-grade logging.

---

##  Key Deliverables

### 1. Penetration Testing & Exploitation
The application was scanned for open ports and services, followed by manual exploitation of input-related vulnerabilities.
- **Service Discovery:** Identified Express.js server running on port 3000 via Nmap.
- **SQL Injection (SQLi):** Successfully bypassed the Administrative login using the payload: `' OR 1=1 --`.
- **Cross-Site Scripting (XSS):** Confirmed reflected XSS vulnerability via the search parameter using a `<script>` alert payload.

### 2. Defensive Logging Implementation
To improve the visibility of the application's security state, I integrated the `winston` library into the server core.
- **Configuration:** Created a logger with dual transports (Console and File).
- **Audit Trail:** All system startup events are now recorded in a persistent `security.log` file for forensic review.

```javascript
// Sample Logger Integration
const winston = require('winston');
const logger = winston.createLogger({
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'security.log' })
  ]
});

Best Practice,Status,Finding
Input Validation,❌ FAILED,Susceptible to SQLi and XSS attacks.
HTTPS Enforcement,❌ FAILED,Traffic transmitted over unencrypted HTTP.
Password Security,✅ PASSED,Credentials stored using Bcrypt salted hashing.

Installation & Usage

Clone the repository:
git clone [https://github.com/](https://github.com/)[YOUR_USERNAME]/juice-shop.git

Install Dependencies:
npm install winston

Start the Application:
npm start
