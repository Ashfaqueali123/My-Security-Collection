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
