# CyberSecurity
Strengthening Security Measures for a Web Application.
Implementing Security Measures
Project Title:

Strengthening Security Measures for a Web Application

Overview:

This repository documents the implementation of essential web application security measures as part of the cybersecurity project:

Strengthening Security Measures for a Web Application

The objective of this phase was to strengthen the security posture of a vulnerable web application by implementing secure coding practices and defensive security mechanisms.

The practical implementation and security analysis were performed using:

OWASP Juice Shop

within a controlled local laboratory environment.

Week 2 Objectives

The primary goals of this phase were:

Implement input validation and sanitization
Secure password storage using hashing
Improve authentication mechanisms
Secure HTTP response headers
Understand practical web application defense techniques
Apply secure coding practices
Security Measures Implemented
1. Input Validation & Sanitization
Purpose

User input validation was implemented to prevent malicious or malformed data from entering the application.

Improper input handling can lead to vulnerabilities such as:

SQL Injection
Cross-Site Scripting (XSS)
Command Injection
Invalid data processing
Validator.js Installation
npm install validator
Example Implementation
const validator = require('validator');

if (!validator.isEmail(email)) {
    return res.status(400).send('Invalid email');
}
Security Benefits
Prevents invalid user input
Reduces injection attack risks
Improves application stability
Protects backend processing logic
2. Password Hashing Using bcrypt
Purpose

Passwords were secured using bcrypt hashing before storage in the database.

Storing plain-text passwords creates severe security risks if the database becomes compromised.

bcrypt Installation
npm install bcrypt
Example Implementation
const bcrypt = require('bcrypt');

const hashedPassword = await bcrypt.hash(password, 10);
Security Benefits
Secure password storage
Resistance against brute-force attacks
Protection against credential leakage
Improved authentication security
3. Token-Based Authentication Using JWT
Purpose

JSON Web Token (JWT) authentication was implemented to improve secure session management and user authentication.

JWT Installation
npm install jsonwebtoken
Example Implementation
const jwt = require('jsonwebtoken');

const token = jwt.sign(
    { id: user._id },
    'your-secret-key'
);

res.send({ token });
Authentication Workflow
User Login
     ↓
Server Verifies Credentials
     ↓
JWT Token Generated
     ↓
Token Sent to Browser
     ↓
Authenticated Access Granted
Security Benefits
Secure authentication
Stateless session management
Protected API access
Improved scalability
4. Securing HTTP Headers Using Helmet.js
Purpose

Helmet.js was implemented to secure HTTP response headers and improve browser-side protection.

Helmet.js Installation
npm install helmet
Example Implementation
const helmet = require('helmet');

app.use(helmet());
Security Headers Added
Header	Purpose
X-Frame-Options	Prevents clickjacking
Content-Security-Policy	Reduces XSS attacks
X-Content-Type-Options	Prevents MIME sniffing
Referrer-Policy	Controls referrer information
Security Benefits
Protects against browser-based attacks
Enhances HTTP security
Reduces information leakage
Improves secure communication
Technologies Used
Technology	Purpose
Node.js	Runtime Environment
npm	Package Management
Express.js	Backend Framework
Angular	Frontend Framework
Validator.js	Input Validation
bcrypt	Password Hashing
JSON Web Token (JWT)	Authentication
Helmet.js	HTTP Security Headers
Git & GitHub	Version Control
Security Improvements Achieved
Security Control	Improvement
Validator.js	Input sanitization and validation
bcrypt	Secure password storage
JWT	Secure authentication and session handling
Helmet.js	Enhanced HTTP response security
Learning Outcomes

Through this phase, the following cybersecurity concepts were understood and implemented:

Secure coding practices
Input validation and sanitization
Password hashing mechanisms
JWT authentication
Session management security
HTTP security headers
Defensive web application security
Ethical Use Notice

This project was conducted strictly within a local and controlled laboratory environment using intentionally vulnerable applications for educational and cybersecurity learning purposes only.

No unauthorized systems or networks were targeted during this project.

References
OWASP Official Website
OWASP Juice Shop Repository
Validator.js Documentation
bcrypt Documentation
jsonwebtoken Documentation
Helmet.js Documentation
