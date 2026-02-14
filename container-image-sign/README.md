# Container Image Sign GitHub Action

This GitHub Action signs container images using **Cosign** (keyless OIDC) and optionally attests a Software Bill of Materials (SBOM) and a CVE report (e.g., Trivy JSON). It is designed for secure, automated image signing and attestation in CI/CD pipelines, especially for Kubernetes and GKE workflows.

## Features

- **Cosign Keyless Signing**: Signs container images using OIDC identity, no key management required.
- **Multi-arch Support**: Optionally sign multi-architecture image indexes recursively.
- **Transparency Log**: Upload signatures and attestations to Rekor transparency log (public or private).
- **SBOM Attestation**: Attest a CycloneDX or SPDX SBOM file to the image.
- **CVE Report Attestation**: Attest a vulnerability report (e.g., Trivy JSON) to the image.
- **Custom Annotations**: Add custom key-value annotations to the signature.

## Inputs

| Name                | Description                                                                 | Required | Default         |
|---------------------|-----------------------------------------------------------------------------|----------|-----------------|
| `image`             | Image reference (use repo@digest)                                           | Yes      | -               |
| `recursive-sign`    | Add `--recursive` to sign multi-arch index & images (`true`/`false`)        | No       | `false`         |
| `tlog-upload`       | Upload signatures/attestations to Rekor transparency log (`true`/`false`)   | No       | `false`         |
| `rekor-url`         | Optional Rekor URL for private instances                                    | No       | (empty)         |
| `extra-annotations` | Additional `--annotation` entries (newline-separated `key=value`)           | No       | (empty)         |
| `cve-report`        | Optional vulnerability report file (e.g., Trivy JSON)                       | No       | (empty)         |
| `attest-cve-report` | Create a cosign attestation for the CVE report (`true`/`false`)             | No       | `true`          |
| `sbom-path`         | Optional SBOM file to attest (CycloneDX XML/JSON or SPDX)                   | No       | (empty)         |
| `sbom-format`       | SBOM format (`cyclonedx`, `spdx`)                                           | No       | `cyclonedx`     |
| `sbom-media-type`   | Override OCI media type; if empty, inferred from format & extension         | No       | (empty)         |
| `use-oci-referrers` | Use OCI 1.1 referrers instead of tags (recommended for GAR "Attachments")   | No       | `true`          |

## Outputs

| Name             | Description                                 |
|------------------|---------------------------------------------|
| `signed-ref`     | Signed image reference (repo@digest)        |
| `cve-attested`   | Whether a CVE attestation was created       |
| `sbom-attested`  | Whether an SBOM attestation was created     |

## Example Workflow

```yaml
name: Container Image Sign

on:
  push:
    branches: [main]

jobs:
  sign:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Sign Container Image
        uses: Unifits/unifits-github-actions/container-image-sign@v1
        with:
          image: 'gcr.io/my-project/my-image@sha256:...'
          recursive-sign: 'true'
          tlog-upload: 'true'
          sbom-path: 'sbom.cdx.json'
          sbom-format: 'cyclonedx'
          cve-report: 'trivy-report.json'
          attest-cve-report: 'true'
```

## Notes

- Uses [Cosign](https://github.com/sigstore/cosign) for signing and attestation.
- Supports keyless signing via OIDC (no private key management).
- Attestations can be used for supply chain security and compliance.
- For Google Artifact Registry (GAR), set `use-oci-referrers: 'true'` to make signatures and SBOMs appear in the **Attachments** (Anhänge) tab instead of as separate image tags.
- Ideal for Kubernetes/GKE and modern DevSecOps workflows.
