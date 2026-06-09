# CyberSecurity1
# Phase 1: API Security Hardening, Rate Limiting & Access Controls

## 📌 Overview
This phase details the engineering and deployment of defensive application controls designed to mitigate automated infrastructure attacks. The primary objective was to configure a robust, defense-in-depth posture inside an Express.js runtime engine to block brute-force disruption, unthrottled request pipelines, and cross-origin data exposure.

## 🛠️ Detailed Technical Implementation

### 1. HTTP Security Architecture (Via Helmet.js)
To remediate weak default server configurations, HTTP header filtering rules were hardened using **Helmet middleware**. This layer injects deterministic transport protections into every HTTP response block:
* **`X-Content-Type-Options: nosniff`**: Completely neutralizes MIME-type sniffing vulnerabilities by forcing browsers to strictly follow the content types defined by the server.
* **`Strict-Transport-Security (HSTS)`**: Implements a strict cryptographic rule forcing user agents to communicate exclusively over secure HTTPS channels for a maximum time-to-live window.
* **`Content-Security-Policy (CSP)`**: Establishes strict contextual boundaries, preventing the execution of unsafe inline scripts and malicious third-party resource loading.

### 2. Traffic Policy Regulation (Rate Limiting)
To defend public endpoints against automated brute-force attacks and intentional resource exhaustion (Denial of Service), a sliding-window throttling mechanism was deployed using `express-rate-limit`:
* **Throttling Window**: Configured to restrict maximum incoming request frequencies within a precise time block per distinct client IP identifier.
* **Fallback Behavior**: Requests crossing this operational baseline are automatically dropped before hitting the application controller layer, returning a standardized `429 Too Many Requests` status code.

### 3. Cross-Origin Control (CORS) Configuration
To block cross-origin script interactions from untrusted domains, standard wildcard origin matching (`*`) was removed and replaced with explicit cross-origin policy parameters:
* Restricted the `Access-Control-Allow-Origin` property to match only approved corporate front-end layout profiles.
* Systematically drops incoming API execution sequences initiated from unlisted domains.

### 4. Cryptographic Key Access Enforcement
A dedicated route protection layer was engineered to safeguard high-value data interfaces:
* **Validation Layer**: A custom middleware scanner inspects the incoming request header for a valid master cryptographic API key.
* **Unauthorized State**: Missing, empty, or malformed key headers generate immediate `401 Unauthorized` JSON responses, dropping the pipeline execution early.
* **Authorized State**: Presenting a validated key allows a clean execution pass, returning structural JSON results alongside a standard `200 OK` validation code.

### 5. Host Perimeter Intrusion Prevention (Fail2Ban)
To extend security outside the node runtime environment, host logs were monitored using **Fail2Ban**:
* Deployed an automated log parser to watch system access points like SSH for malicious connection behavior.
* Upon detecting consecutive authentication failures, Fail2Ban modifies the host-level kernel firewall rules dynamically, dropping any traffic from the attacking IP address inside the active `sshd jail`.

## 📊 Verification Metrics
* **Policy Rate Check**: High-velocity automated connection bursts successfully trigger HTTP `429` statuses, verifying that the traffic regulation logic behaves exactly as intended.
* **Authentication Split**: Invalid token request passes are consistently dropped with `401` errors, while verified keys maintain stable `200 OK` loops.
* **Jail Execution**: Terminal log audits verify real-time perimeter blocklisting tracking, showing attacking entities being successfully isolated by the firewall.
=======
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
>>>>>>> cyber2/main
=======
# Phase 3: Offensive Validation, Burp Suite, Metasploit Analysis & Code Remediation

## 📌 Overview
This phase completes the security engineering lifecycle by connecting offensive exploit validation with immediate source-code remediation. A severe input-validation vulnerability was isolated inside a test application runtime route (running Python and an SQLite database on port `5000`). The endpoint was analyzed using Burp Suite traffic proxies, validated via the **Metasploit Framework** to simulate real-world exploit impact, and permanently neutralized by refactoring the backend codebase to use parameterized queries.

## 🛠️ Detailed Technical Implementation

### 1. Traffic Interception & Inspection (Burp Suite)
To analyze incoming request flows and trace how client-supplied input vectors are processed by the backend, traffic was routed through an interception proxy:
* Configured local browser proxy parameters to capture, logs, and review live HTTP requests passing to target port `5000`.
* Performed manual parameter fuzzing by embedding explicit database control sequences (such as `'`, `"`, and `--`) into input parameters to look for syntax anomalies, unhandled database errors, and unstable response behavior.

### 2. Offensive Validation & Penetration Testing (Metasploit Framework)
Following manual parameter tracking, the **Metasploit Framework (MSF)** was deployed to evaluate the attack surface and simulate advanced exploitation phases:
* **Auxiliary Mapping**: Deployed specialized Metasploit auxiliary modules to perform targeted fuzzing, service profiling, and parameter exposure scanning against the active web target interface.
* **Exploit Verification**: Leveraged specific payload delivery configurations to evaluate runtime boundaries, verify structural data extraction potential, and document how arbitrary strings interact with backend database layers.
* **Vulnerability Analysis**: Review of the backend exception traces revealed that inserting raw, unvalidated client data strings directly into the SQL command template broke the intended query execution boundaries, triggering unhandled SQLite operational exceptions that could lead to data extraction or server instability.

### 3. Source-Code Hardening (Parameterized Query Engineering)
To resolve this vulnerability permanently, the application's source code was opened in a terminal editing session and refactored to replace the insecure string-concatenation pattern with defensive coding standards:

#### ❌ Vulnerable Implementation Pattern (Pre-Patch)
```python
# INSECURE: Directly merging raw user text into the SQL string template
query = "SELECT * FROM products WHERE id = '" + user_input + "'"
cursor.execute(query)
>>>>>>> cyber3/main
