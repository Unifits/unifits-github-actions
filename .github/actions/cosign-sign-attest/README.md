# Cosign Sign & Attest Action

## Overview
This GitHub Action signs a container image using **Cosign** (keyless signing via GitHub OIDC) and creates two attestations:

- **Provenance Attestation** (SLSA-compliant build metadata)
- **CVE Report Attestation** (based on a vulnerability scan report, e.g., Trivy JSON)

This helps ensure **supply chain security** and compliance with modern security standards.

---

## Features
✔ Keyless signing using GitHub OIDC (no secrets required)  
✔ Generates a provenance predicate automatically  
✔ Attests both provenance and vulnerability scan results  
✔ Works with any OCI-compliant image registry

---

## Inputs
| Name        | Description                                      | Required |
|-------------|--------------------------------------------------|----------|
| `image`     | Full image reference with digest (e.g., `registry/repo@sha256:...`) | ✅ |
| `cve-report`| Path to CVE report JSON file (e.g., `trivy-report.json`)           | ✅ |

---

## Example Usage
```yaml
jobs:
  security-signing:
    runs-on: ubuntu-latest
    permissions:
      id-token: write   # Required for keyless signing
      contents: read
    steps:
      - uses: actions/checkout@v3

      # Example: Build image and generate Trivy report before signing
      - name: Build Image
        run: |
          echo "Build and push your image here"
          echo "sha256:digest" > digest.txt

      - name: Run Trivy Scan
        uses: aquasecurity/trivy-action@0.33.1
        with:
          image-ref: my-registry/my-image@sha256:digest
          format: cosign-vuln
          output: trivy-report.json
          severity: CRITICAL

      # Use the Cosign Sign & Attest Action
      - name: Cosign Sign & Attest
        uses: unifits-github-actions/cosign-sign-attest@v1
        with:
          image: my-registry/my-image@sha256:digest
          cve-report: trivy-report.json
```

---

## What It Does
1. Installs **Cosign**.
2. Signs the image using **keyless signing**.
3. Generates a **provenance.json** file with build metadata.
4. Creates two attestations:
    - **Provenance** (`--type slsaprovenance`)
    - **CVE Report** (`--type vuln`)

---

## Requirements
- GitHub Actions workflow must include:
  ```yaml
  permissions:
    id-token: write
  ```
- Cosign and Trivy installed via actions (handled automatically in this example).
- Image must be pushed to a registry before signing.

---

## Security Notes
- Keyless signing uses GitHub OIDC tokens; no private keys or secrets required.
- Attestations are stored in the same OCI registry as the image.

---