
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

## Inputs

| Name                | Description                                                        | Required | Default         |
|---------------------|--------------------------------------------------------------------|----------|-----------------|
| `maven-profile`     | Maven profile to use for dockerization                            | No       | `dockerize`     |
| `digest-output-file`| Filename used by Jib to write the built image digest              | No       | `${{ github.workspace }}/digest.txt` |
| `maven-goals`       | Maven goals to execute (space-separated)                          | No       | `deploy`        |
| `batch-mode`        | Use Maven batch mode (`true`/`false`)                             | No       | `true`          |
| `update-snapshots`  | Pass `--update-snapshots` flag (`true`/`false`)                  | No       | `true`          |
| `extra-args`        | Optional additional Maven arguments (e.g., `-DskipTests`)         | No       | (empty)         |

## Outputs

| Name    | Description                              |
|---------|------------------------------------------|
| `digest`| Image digest (sha256:…) produced by Jib  |

## Example Workflow

```yaml
name: Maven Jib Build & Push

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build and Push Docker Image
        uses: Unifits/unifits-github-actions/maven-jib-build@v1
        with:
          maven-profile: 'dockerize'
          maven-goals: 'deploy'
          batch-mode: 'true'
          update-snapshots: 'true'
```

## Notes

- Uses Maven and Jib for building and pushing container images.
- The image digest can be used for subsequent steps, such as signing or deployment.
- Supports advanced Maven configurations via extra arguments.
