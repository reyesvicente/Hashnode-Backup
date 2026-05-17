---
title: "Post-Mortem: The March 2026 Axios Supply Chain Attack"
datePublished: 2026-03-31T11:37:45.661Z
cuid: cmnejlx2y005r2dn6b6o2c42j
slug: post-mortem-the-march-2026-axios-supply-chain-attack
cover: https://cdn.hashnode.com/uploads/covers/5f95116240346172a86c20c6/49c03fb3-e056-497d-bb85-dbadb2e8941d.png
ogImage: https://cdn.hashnode.com/uploads/og-images/5f95116240346172a86c20c6/283d1489-9968-4945-b57d-f0e302e594b2.png
tags: javascript, security, nodejs, webdev

---

## The Incident

On March 31, 2026, a high-profile supply chain attack targeted **Axios**, a critical HTTP client for the JavaScript ecosystem. By hijacking a maintainer's NPM account, attackers injected a malicious dependency, `plain-crypto-js`, which deployed a cross-platform Remote Access Trojan (RAT).

* * *

## Incident Summary

<table style="min-width: 50px;"><colgroup><col style="min-width: 25px;"><col style="min-width: 25px;"></colgroup><tbody><tr><td colspan="1" rowspan="1"><p><strong>Detail</strong></p></td><td colspan="1" rowspan="1"><p><strong>Information</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Affected Versions</strong></p></td><td colspan="1" rowspan="1"><p><code>axios@1.14.1</code>, <code>axios@0.30.4</code></p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Malicious Dependency</strong></p></td><td colspan="1" rowspan="1"><p><code>plain-crypto-js@4.2.1</code></p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Payload</strong></p></td><td colspan="1" rowspan="1"><p>Cross-platform RAT (Linux, macOS, Windows)</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>C2 Server</strong></p></td><td colspan="1" rowspan="1"><p><code>sfrclak.com:8000</code></p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Resolution Window</strong></p></td><td colspan="1" rowspan="1"><p>Live for ~3 hours (00:21 – 03:29 UTC)</p></td></tr></tbody></table>

* * *

## Technical Deep Dive

The attack bypassed standard security audits by hiding the malicious logic within a sub-dependency. Once installed via a standard `npm install`, the payload scanned the host machine for:

*   **Environment Variables:** `.env` files and active shell exports.
    
*   **Auth Tokens:** `~/.npmrc` and `~/.aws/credentials`.
    
*   **SSH Keys:** Unprotected private keys in `~/.ssh`.
    

Data was exfiltrated via **POST requests** to the [`sfrclak.com`](http://sfrclak.com) Command & Control (C2) server.

* * *

## Remediation & Verification

To ensure a development environment is sanitized, the following protocol was executed:

1.  **Network Sinkholing:** Manually mapping the C2 domain to `127.0.0.1` in `/etc/hosts` to prevent further exfiltration and "kill" the phone-home capability.
    
2.  **Lockfile Audit:** Scanning all local projects for traces of the malicious package using a space-safe search:
    
    Bash
    
    ```shell
    find . -type f \( -name "package-lock.json" -o -name "yarn.lock" \) -print0 | xargs -0 grep "plain-crypto-js"
    ```
    
3.  **Environment Sanitization:** Clearing the global NPM cache and updating tool managers (like `mise`) to ensure only verified versions are used moving forward.
    

> \[!TIP\]
> 
> **Pro-Tip:** Always use `npm audit` or tools like **Snyk** to monitor your dependency tree for "hidden" sub-dependencies that do not appear directly in your `package.json`.