
# Container Image Scan

**Description:**  
Generate Trivy HTML report and JSON CVE report for a container image.  
This composite GitHub Action scans a container image for vulnerabilities using Trivy, produces a human-readable HTML report, and generates a JSON CVE report suitable for further processing (e.g., Cosign attestation).

---

## 🚀 Features

- Downloads the official Trivy HTML template for reporting.
- Runs Trivy to scan the specified container image and outputs:
    - An HTML vulnerability report (artifact).
    - A JSON CVE report (for automated security workflows).
- Uploads the HTML report as a workflow artifact.
- Fails the workflow if critical vulnerabilities are found (in the JSON report step).

---

## 📥 Inputs

| Name         | Description                                                                 | Required | Default             |
|--------------|-----------------------------------------------------------------------------|----------|---------------------|
| `image`      | Image reference with digest (e.g., `registry/repo@sha256:...`)              | Yes      | —                   |
| `html-report`| Path for HTML report output                                                 | No       | `trivy-report.html` |
| `json-report`| Path for JSON report output                                                 | No       | `trivy-report.json` |

---

## 📤 Outputs

This action does not define explicit outputs, but it produces two files:
- The HTML report (uploaded as an artifact).
- The JSON report (saved to disk for use in subsequent steps).

---

## 📝 Usage Example

```yaml
- name: Container Image Scan
  uses: ./.github/actions/container-image-scan
  with:
    image: europe-west3-docker.pkg.dev/unifits-artifacts-shared/unifits-docker/unifits/plugin-repository@sha256:...
    html-report: trivy-report.html
   

