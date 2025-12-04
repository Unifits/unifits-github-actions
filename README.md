
# UNIFITS GitHub Actions

Welcome to the UNIFITS GitHub Actions repository!  
This repository provides a collection of reusable, composable GitHub Actions for secure, efficient, and automated CI/CD workflows—tailored for Java, container, and cloud-native projects.

---

## 🚀 What’s Inside?

- **Container Security:**  
  Actions for scanning container images with Trivy and signing/attesting images with Cosign.

- **Java Build Automation:**  
  Composite actions for building and pushing container images using Maven and Jib.

- **Caching:**  
  Actions to restore and save Maven dependencies to Google Cloud Storage, speeding up builds and reducing network usage.

- **Best Practices:**  
  All actions are designed for security, reproducibility, and easy integration into modern CI/CD pipelines.

---

## 📦 Action Directory

Each action is located in its own subdirectory under `.github/actions/` and includes:

- `action.yml` — Action definition and schema.
- `README.md` — Documentation and usage examples.
- Supporting scripts or templates as needed.

---

## 📝 Usage Example

To use an action from this repository in your workflow:

```yaml
- name: Example: Maven Jib Build & Push
  uses: ./.github/actions/maven-jib-build
  with:
    maven-profile: dockerize
    digest-output-file: digest.txt
    maven
