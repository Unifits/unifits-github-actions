
# Container Image Scan

**Description:**  
Generate Trivy HTML and JSON CVE reports for a container image, with configurable failure behavior based on vulnerability findings.

This composite GitHub Action scans a container image for vulnerabilities using Trivy, produces a human-readable HTML report, and generates a machine-readable JSON CVE report. You can choose whether the workflow should fail when vulnerabilities are found at or above a specified severity threshold.

---

## 🚀 Features

- Downloads the official Trivy HTML template for reporting.
- Scans container images for OS and library vulnerabilities.
- Outputs:
  - An HTML vulnerability report (uploaded as a workflow artifact).
  - A JSON CVE report (for automated security workflows).
- Configurable failure behavior:
  - By default, fails the workflow if vulnerabilities at or above the threshold are found.
  - Can be set to continue even if vulnerabilities are detected.
- Outputs a boolean flag (`failed`) for conditional downstream logic.

---

## 📥 Inputs

| Name              | Description                                                                                      | Required | Default             |
|-------------------|--------------------------------------------------------------------------------------------------|----------|---------------------|
| `image`           | Image reference with digest (e.g., `registry/repo@sha256:...`)                                   | Yes      | —                   |
| `html-report`     | Path for HTML report output                                                                      | No       | `trivy-report.html` |
| `json-report`     | Path for JSON report output                                                                      | No       | `trivy-report.json` |
| `fail-on`         | Whether the step should fail when vulnerabilities are found at or above the configured severity (`true`/`false`). | No       | `true`              |
| `fail-on-severity`| Severity threshold at which a failure should be triggered (`LOW`, `MEDIUM`, `HIGH`, `CRITICAL`). | No       | `CRITICAL`          |
| `ignore-unfixed`  | Ignore vulnerabilities that are not yet fixed (`true`/`false`).                                  | No       | `true`              |

---

## 📤 Outputs

| Name    | Description                                                                 |
|---------|-----------------------------------------------------------------------------|
| `failed`| Boolean flag indicating if vulnerabilities at or above the threshold were found.|

---

## 📝 Usage Example

**Default (fail on CRITICAL):**
```yaml
- name: Image Scan
  uses: ./.github/actions/container-image-scan
  with:
    image: ${{ env.GAR_IMAGE }}@${{ steps.build.outputs.digest    image: ${{ env.GAR_IMAGE }}@${{ steps.build.outputs.digest }}
