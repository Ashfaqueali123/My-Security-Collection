# CyberSecurity2
# Phase 2: Automated Infrastructure Mapping, Directory Auditing & Security Assessments

## 📌 Overview
This phase focuses on automated black-box reconnaissance, infrastructure mapping, and configuration auditing against an enterprise sandbox target deployment environment running **OWASP WebGoat**. The target service was initialized locally on port `8080` to act as a control baseline for assessing network attack surfaces and analyzing vulnerabilities.

## 🛠️ Detailed Technical Implementation

### 1. Network Profile Mapping (Nmap)
A targeted network discovery audit was executed using **Nmap** to inventory visible entry points and fingerprint active listeners:
* **Mechanics**: Ran advanced service variation scans (`-sV`) combined with default scanning scripts (`-sC`) to identify active daemon interfaces.
* **Output**: Successfully isolated port `8080/tcp` as open and mapped the underlying HTTP proxy structures and header flags exposed by the container workspace.

### 2. Path & Endpoint Fuzzing (Gobuster)
To catalog hidden directories and administration routes that standard web crawlers miss, a directory fuzzing sequence was executed using **Gobuster**:
* **Mechanics**: Ran recursive wordlist lookup strategies targeting the application root URL path.
* **Output**: Identified active internal endpoints, including administrative subpaths, account registration routes, and active login pages returning explicit HTTP `200` validation parameters.

### 3. Automated Configuration Auditing (Nikto)
A targeted vulnerability audit was run using the **Nikto** server scanning suite to isolate software configuration issues and missing perimeter flags:
* **Flagged Anomalies**: Identified exposed server build banners, structural information leaks, and missing defensive headers.
* **Core Flaws**: Documented the complete absence of anti-framing defenses (`X-Frame-Options` missing) and missing `X-Content-Type-Options` security parameters across multiple web directories.

### 4. Interactive Vulnerability Proxy Spiders (OWASP ZAP)
To evaluate application parameters and input vectors, an active vulnerability assessment was run using **OWASP ZAP**:
* **Mechanics**: Configured the target application traffic to route through the ZAP daemon proxy engine. 
* **Execution**: Ran active web spidering and fuzzing passes to evaluate input fields against common exploit patterns.
* **Classification**: Successfully cataloged security findings into a prioritized risk dashboard ranging from Informational to Low and Medium priority alert logs.

## 📊 Discovered Gaps & Remediation Matrix

| Target Flaw | Found Path | Severity | Exploit Scenario | Hardening Action |
| :--- | :--- | :---: | :--- | :--- |
| **Missing `X-Frame-Options`** | Application Root | **Medium** | Clickjacking/UI Redressing attacks forcing malicious form submissions. | Inject `X-Frame-Options: DENY` header policy inside the global middleware controller. |
| **Missing `X-Content-Type-Options`** | Global API Routes | **Low** | Cross-Site Scripting (XSS) via MIME-type sniffing on text resources. | Append `nosniff` configuration parameters across all response pipelines. |
| **Exposed Management Path Nodes** | `/registration`, `/login` | **High** | Targeted brute-force attacks and brute-force configuration enumeration. | Restrict endpoint availability behind explicit access lists or a corporate VPN ring. |

## 📝 Assessment Deliverable
All raw logs, tool traces, and tool outputs were parsed and compiled into an interactive, structured HTML security report. This file provides summary charts, risk breakdown metrics, and clear, actionable technical remediations to guide ongoing patching efforts.
