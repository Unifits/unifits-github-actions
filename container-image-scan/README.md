# Container Image Scan GitHub Action

This GitHub Action scans container images using **Trivy**, generates vulnerability reports (HTML and machine-readable), optionally fails builds based on severity, and creates an SBOM for attestation. It is ideal for CI/CD pipelines focused on security and compliance, including GKE deployments.

## Features

- **Trivy Scan**: Automatic security scan for container images (OS & libraries)
- **Report Generation**: HTML report (optional) and machine-readable vulnerability report (optional, multiple formats)
- **Build Policy**: Fail pipeline if vulnerabilities above a defined severity are found
- **SBOM Generation**: Software Bill of Materials (CycloneDX or SPDX)
- **GCS Cache**: Use Google Cloud Storage bucket for Trivy cache (performance boost)
- **Flexible Configuration**: Multiple parameters for customization

## Inputs

| Name               | Description                                                              | Required | Default            |
|--------------------|--------------------------------------------------------------------------|----------|--------------------|
| `image`            | Image reference with digest (e.g., `registry/repo@sha256:...`)          | Yes      | -                  |
| `html-enable`      | Generate HTML report (`true`/`false`)                                   | No       | `true`             |
| `vuln-enable`      | Generate machine-readable vulnerability report (`true`/`false`)         | No       | `true`             |
| `vuln-format`      | Vulnerability report format (`cosign-vuln`, `json`, `sarif`)            | No       | `cosign-vuln`      |
| `fail-on`          | Fail build on vulnerabilities above severity threshold (`true`/`false`) | No       | `true`             |
| `fail-on-severity` | Severity threshold (`LOW`, `MEDIUM`, `HIGH`, `CRITICAL`)                | No       | `CRITICAL`         |
| `ignore-unfixed`      | Ignore unfixed vulnerabilities (`true`/`false`)                          | No       | `true`             |
| `sbom-enable`         | Generate SBOM (`true`/`false`)                                          | No       | `true`             |
| `sbom-format`         | SBOM format (`cyclonedx-json`, `spdx-json`)                             | No       | `cyclonedx-json`   |
| `html-artifact-name`  | Artifact name for the HTML report                                       | No       | `trivy-html-report`|
| `html-artifact-prefix`| Optional prefix for the HTML artifact name                              | No       | (empty)            |
| `sbom-artifact-name`  | Artifact name for the SBOM                                              | No       | `sbom`             |
| `sbom-artifact-prefix`| Optional prefix for the SBOM artifact name                              | No       | (empty)            |
| `gcs-bucket`          | GCS bucket for Trivy cache                                              | Yes      | -                  |
| `gcs-path-prefix`  | Prefix in bucket (e.g., `trivy-cache`)                                  | No       | `trivy-cache`      |
| `fixed-cache-key`  | Fixed restore key (e.g., `trivy-db-current` or `trivy-db-YYYY-MM-DD`)   | Yes      | -                  |

## Outputs

| Name               | Description                                                              |
|--------------------|--------------------------------------------------------------------------|
| `failed`           | Boolean flag: Were vulnerabilities above severity threshold found?      |
| `json-report-path` | Path to machine-readable vulnerability report                           |
| `html-report-path` | Path to HTML report                                                     |
| `sbom-path`        | Path to generated SBOM                                                  |
| `sbom-format`      | SBOM format (`cyclonedx` or `spdx`)                                     |

## Example Workflow

```yaml
name: Container Image Scan

on:
  push:
    branches: [main]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Container Image Scan
        uses: Unifits/unifits-github-actions/container-image-scan@v1
        with:
          image: 'gcr.io/my-project/my-image@sha256:...'
          gcs-bucket: 'my-trivy-cache-bucket'
          fixed-cache-key: 'trivy-db-current'
```

## Notes

- Uses [Trivy](https://github.com/aquasecurity/trivy) for scanning and SBOM generation.
- GCS cache significantly speeds up repeated scans.
- Reports and SBOM can be uploaded as build artifacts.
- Ideal for Kubernetes/GKE workflows.
