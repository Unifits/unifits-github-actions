
# Cosign Sign & Attest

**Description:**  
Digitally sign and attest container images using Cosign, leveraging Trivy JSON CVE reports for vulnerability evidence.  
This composite GitHub Action signs a container image and attaches a vulnerability attestation, ensuring supply chain integrity and compliance with security policies.

---

## 🚀 Features

- Signs container images using Cosign.
- Attests images with a Trivy-generated JSON CVE report.
- Integrates seamlessly into CI/CD pipelines for automated supply chain security.

---

## 📥 Inputs

| Name         | Description                                                                 | Required | Default             |
|--------------|-----------------------------------------------------------------------------|----------|---------------------|
| `image`      | Image reference with digest (e.g., `registry/repo@sha256:...`)              | Yes      | —                   |
| `cve-report` | Path to Trivy JSON CVE report for attestation                               | Yes      | —                   |

---

## 📤 Outputs

This action does not define explicit outputs, but it produces signed and attested container images in your registry.

---

## 📝 Usage Example

```yaml
- name: Cosign Sign & Attest
  uses: ./.github/actions/cosign-sign-attest
   with:
    image: europe-west3-docker.pkg.dev/unifits-artifacts-shared/unifits-docker/unifits/plugin-repository@sha256:...
