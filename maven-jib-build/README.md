
# Maven Jib Build & Push

**Description:**  
Builds and pushes a container image using Maven and Jib, then outputs the image digest.  
This composite GitHub Action streamlines Java containerization workflows by automating Maven builds with Jib, supporting custom profiles, goals, and additional Maven arguments.

## Features

- **Maven Jib Build**: Builds and pushes container images using Jib and Maven.
- **Profile & Goals**: Supports custom Maven profiles and goals for flexible builds.
- **Digest Output**: Emits the image digest (sha256:…) for downstream steps.
- **Batch Mode & Snapshot Update**: Supports Maven batch mode and snapshot updates.
- **Extra Arguments**: Allows additional Maven arguments for advanced customization.

- Runs Maven with a configurable profile and goals.
- Pushes container images using Jib.
- Captures and outputs the image digest (sha256).
- Supports extra Maven arguments for advanced customization.

| Name                | Description                                                        | Required | Default         |
|---------------------|--------------------------------------------------------------------|----------|-----------------|
| `maven-profile`     | Maven profile to use for dockerization                            | No       | `dockerize`     |
| `digest-output-file`| Filename used by Jib to write the built image digest              | No       | `digest.txt`    |
| `maven-goals`       | Maven goals to execute (space-separated)                          | No       | `deploy`        |
| `batch-mode`        | Use Maven batch mode (`true`/`false`)                             | No       | `true`          |
| `update-snapshots`  | Pass `--update-snapshots` flag (`true`/`false`)                  | No       | `true`          |
| `extra-args`        | Optional additional Maven arguments (e.g., `-DskipTests`)         | No       | (empty)         |

## Outputs

| Name               | Description                                                                 | Required | Default             |
|--------------------|-----------------------------------------------------------------------------|----------|---------------------|
| `maven-profile`    | Maven profile to use for dockerization.                                     | No       | `dockerize`         |
| `digest-output-file`| Filename used by Jib to write the built image digest.                      | No       | `digest.txt`        |
| `maven-goals`      | Maven goals to execute (space-separated).                                   | No       | `deploy`            |
| `batch-mode`       | Use Maven batch mode (`true`/`false`).                                      | No       | `true`              |
| `update-snapshots` | Pass `--update-snapshots` flag (`true`/`false`).                           | No       | `true`              |
| `extra-args`       | Optional additional Maven arguments appended to the command (e.g., `-DskipTests -Drevision=1.2.3`). | No | `""`                |

---

## 📤 Outputs

| Name    | Description                                  |
|---------|----------------------------------------------|
| `digest`| Image digest (sha256:…) produced by Jib.     |

---

## 📝 Usage Example

```yaml
- name: Maven Jib Build & Push
  uses: ./.github/actions/maven-jib-build
  with:
    maven-profile: dockerize
    digest-output-file: digest.txt
    maven-goals: deploy
    batch-mode: true
    update-snapshots: true
    extra-args: "-DskipTests -Drevision=${{ github.sha }}"
