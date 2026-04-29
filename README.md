![nLink Logo](logo.png)

# nLink Workflow System Architecture & Overview Documentation

**Table of Contents**
{{toc}}

## 1. Project Overview

**nLink Workflow** is a dedicated enterprise Workflow Automation Platform. The system provides an intuitive Drag & Drop visual interface (Visual Node-based Engine), enabling users to connect APIs, process complex data, and automate tasks with little to no programming skills (Low-code/No-code).

* **Core Objective:** To provide a flexible, highly secure, and enterprise-grade workflow automation solution, specifically tailored for On-Premise deployments, seamless API orchestration, and intellectual property protection (DRM).
* **Languages & Technologies:** Golang (Backend Engine), Vue.js/Nuxt 3 (Frontend), WebAssembly (WASM for Custom Nodes).

---

## 2. Core Components

### 2.1. Visual Canvas & Debugger
* **Drag & Drop Interface:** Users can freely design workflows by connecting Nodes.
* **Visual Debugger:** A visual debugging tool featuring "Time-Travel", allowing users to inspect the Input/Output details of each Node at every execution step without blocking performance (Non-blocking).

### 2.2. Diverse Node Ecosystem
1. **Community Nodes:** Built-in integration Nodes for popular platforms (Google Sheets, Discord, CRMs, Webhooks, etc.).
2. **Virtual Nodes (Sub-workflows):** A special feature allowing an entire complex workflow to be packaged into a single, reusable Node.
3. **WASM Nodes:** Support for developers to write Nodes in Rust/Go, compile them to WebAssembly, and execute them safely and isolated at native speeds within the wazero runtime.

### 2.3. Dynamic API & Credential Builder
* A dynamic API builder and Credential management system completely separated from the Workflow JSON structure. All API Keys and Tokens are encrypted and stored independently in the user's database, ensuring no credentials leak during workflow exports.

---

## 3. Security Architecture & Enterprise Readiness

### 3.1. Soft DRM (Digital Rights Management) Mechanism
* **Template Marketplace** system enabling Creators to package and commercialize Workflows/Virtual Nodes.
* **Piracy Protection:** When a workflow is flagged with `"is_drm_protected": true`, end-users can import and run it normally, but are permanently restricted from Exporting, Downloading, Duplicating, or extracting it as a Virtual Node.
* **Zero-Day Patch:** Patched vulnerabilities involving REST API interference (preventing users from stripping the DRM flag via API Update requests).

### 3.2. Resource Exhaustion Prevention (OOM & DoS Protection)
* Strict implementation of `MaxBytesReader` and `io.LimitReader` across the entire system:
  * **OAuth2 Callbacks:** Payload limited to 1MB.
  * **Execution Outputs (Workflow Logs):** In-memory parsing limited to 15MB.
  * **Webhooks & Imports:** Payload limited to 5MB for Webhooks and 10MB for Import files.
  * **Zip/Tar Uploads:** Blocked uploads exceeding 50MB and integrated deep scanning mechanisms against Path Traversal (Zip Slip / Tar Bomb).

### 3.3. Enterprise Identity
* Supported authentication via **SAML 2.0 Single Sign-On (SSO)** integrated with Okta, Azure AD, and Google Workspace.
* Zero-Trust Authorization (Just-in-Time Provisioning): New users authenticated via SSO are granted the `VIEWER` role by default.

---

## 4. Deployment & Databases

The system is designed for easy Self-Hosting using Docker (available via Docker Hub `nlinkio/nlink-workflow`).

* **Supported Databases:**
  * MySQL
  * PostgreSQL
  * SQLite (For Embedded/Test versions)
* **Caching & Queueing:** Redis (Supporting millions of parallel tasks with near-zero latency).

---

## 5. Current Status

* **Version:** v1.0.0 (Production Ready)
* **Recently Completed Items:**
  * Released the Template Marketplace Core feature.
  * Tested & Optimized Responsive UI/UX across all devices.
  * Audited the entire system to prevent memory leaks (Memory Leak / OOM).

## 6. Future Roadmap
* Optimize Quota Management based on License tiers (FREE, PRO, ENTERPRISE).
