# Unifits GitHub Actions

A collection of professional GitHub Actions for secure, efficient, and cloud-native CI/CD workflows. These actions are designed to support modern development pipelines, especially for teams using Google Cloud, Maven, and containerized deployments.

## Available Actions

- **Container Image Scan**  
  Scan container images for vulnerabilities using Trivy, generate reports, and create SBOMs.  
  → [./container-image-scan/README.md](./container-image-scan/README.md)

- **Container Image Sign**  
  Sign container images with Cosign (keyless OIDC) and optionally attest SBOMs and CVE reports.  
  → [./container-image-sign/README.md](./container-image-sign/README.md)

- **Maven Cache Restore (GCS)**  
  Restore the Maven local repository cache from Google Cloud Storage to speed up builds.  
  → [./maven-cache-restore/README.md](./maven-cache-restore/README.md)

- **Maven Cache Save (GCS)**  
  Prune SNAPSHOT artifacts and save the Maven repository cache to Google Cloud Storage.  
  → [./maven-cache-save/README.md](./maven-cache-save/README.md)

- **Maven Jib Build & Push**  
  Build and push container images using Maven and Jib, emitting the image digest for downstream steps.  
  → [./maven-jib-build/README.md](./maven-jib-build/README.md)

## Usage

Each action is self-contained and documented in its respective directory. See the individual README.md files for setup, configuration, and example workflows.

## Requirements

- GitHub Actions runner (Linux recommended)
- Google Cloud credentials (for GCS and GAR actions)
- Maven (for Java build actions)
- Docker or Jib (for container build actions)

## License

This repository is licensed under the MIT License.
