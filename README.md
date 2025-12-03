# Unifits GitHub Actions Repository

## Overview
This repository provides reusable **Composite GitHub Actions** to standardize security and compliance workflows across all Unifits projects.

Our goal is to simplify the integration of container image security scanning, signing, and attestation into CI/CD pipelines.

---

## Available Actions

### 1. **Cosign Sign & Attest**
- **Purpose:** Sign container images using Cosign (keyless via GitHub OIDC) and create attestations:
  - Provenance (SLSA-compliant build metadata)
  - CVE Report (based on vulnerability scan)
- **Path:** `.github/actions/cosign-sign-attest`
- **Key Features:**
  - Keyless signing (no secrets required)
  - Generates provenance.json automatically
  - Attests CVE report for supply chain security

### 2. **Trivy Image Scan**
- **Purpose:** Scan container images for vulnerabilities using Trivy and generate:
  - HTML report (human-readable)
  - JSON report (cosign-vuln format for attestation)
- **Path:** `.github/actions/trivy-image-scan`
- **Key Features:**
  - Downloads official Trivy HTML template
  - Uploads HTML report as artifact
  - Fails workflow on CRITICAL vulnerabilities

---

## Quick Start

### Using Cosign Sign & Attest
```yaml
- name: Cosign Sign & Attest
  uses: unifits-github-actions/cosign-sign-attest@v1
  with:
    image: my-registry/my-image@sha256:digest
    cve-report: trivy-report.json
```

### Using Trivy Image Scan
```yaml
- name: Trivy Image Scan (HTML + JSON)
  uses: unifits-github-actions/trivy-image-scan@v1
  with:
    image: my-registry/my-image@sha256:digest
    html-report: trivy-report.html
    json-report: trivy-report.json
```

---

## Versioning
- Actions are versioned using Git tags and GitHub Releases.
- Use floating tags like `@v1` for stable major versions.
- Example:
  ```bash
  git tag v1.0.0
  git tag v1
  git push origin v1.0.0 v1
  ```

---

## Requirements
- GitHub workflow must include:
  ```yaml
  permissions:
    id-token: write
  ```
- Internet access for downloading Trivy template.
- Image must be pushed to a registry before scanning/signing.
