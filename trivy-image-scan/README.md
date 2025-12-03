# Trivy Image Scan Action

## Overview
This GitHub Composite Action performs a vulnerability scan on a container image using **Trivy** and generates:

- **HTML Report** for human-readable vulnerability details.
- **JSON Report** (cosign-vuln format) for attestation and automated security workflows.

It is designed to integrate easily into CI/CD pipelines for container security.

---

## Features
✔ Downloads official Trivy HTML template automatically  
✔ Generates HTML vulnerability report and uploads it as an artifact  
✔ Generates JSON CVE report for Cosign attestation  
✔ Fails the workflow if CRITICAL vulnerabilities are found

---

## Inputs
| Name          | Description                                      | Required |
|---------------|--------------------------------------------------|----------|
| `image`       | Full image reference with digest (e.g., `registry/repo@sha256:...`) | ✅ |
| `html-report` | Path for HTML report output (default: `trivy-report.html`)         | ❌ |
| `json-report` | Path for JSON report output (default: `trivy-report.json`)         | ❌ |

---

## Example Usage
```yaml
jobs:
  trivy-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      # Run Trivy Image Scan Action
      - name: Trivy Image Scan (HTML + JSON)
        uses: unifits-github-actions/trivy-image-scan@v1
        with:
          image: my-registry/my-image@sha256:digest
          html-report: trivy-report.html
          json-report: trivy-report.json
```

---

## What It Does
1. Downloads the official Trivy HTML template.
2. Generates an HTML vulnerability report using `aquasecurity/trivy-action`.
3. Uploads the HTML report as an artifact.
4. Generates a JSON CVE report in `cosign-vuln` format for attestation.
5. Fails the workflow if CRITICAL vulnerabilities are detected.

---

## Requirements
- Image must be pushed to a registry before scanning.
- GitHub Actions runner must have internet access to download the template.

---

## Outputs
- HTML report uploaded as an artifact.
- JSON report available for Cosign attestation.

---