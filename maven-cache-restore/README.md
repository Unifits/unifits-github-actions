# Maven Cache Restore (GCS) GitHub Action

This GitHub Action restores the Maven local repository cache from Google Cloud Storage (GCS). It is designed to speed up Maven builds in CI/CD pipelines by reusing previously cached dependencies, reducing build times and network usage.

## Features

- **Restore Maven Cache**: Downloads the Maven `.m2/repository` cache from a specified GCS bucket.
- **Customizable Keys and Paths**: Supports custom cache keys, GCS bucket paths, and local repository locations.
- **Optimized for CI/CD**: Ideal for workflows running on GitHub Actions with Google Cloud integration.

## Inputs

| Name             | Description                                                                 | Required | Default                                               |
|------------------|-----------------------------------------------------------------------------|----------|-------------------------------------------------------|
| `cache-key`      | Primary cache key for the Maven repository (e.g., `maven-<os>-<repo>-<hash>`) | No       | `maven-${{ runner.os }}-${{ github.repository }}-${{ hashFiles('**/pom.xml') }}` |
| `gcs-bucket`     | Name of the GCS bucket that stores the cache                                | Yes      | -                                                     |
| `gcs-path-prefix`| Path prefix within the bucket (e.g., `maven/<repo>/releases`)               | No       | `maven/${{ github.repository }}/releases`             |
| `path`           | Local path to the Maven repository                                          | No       | `~/.m2/repository`                                    |

## Outputs

| Name                | Description                                      |
|---------------------|--------------------------------------------------|
| `cache-primary-key` | The primary key used for the restored cache      |

## Example Workflow

```yaml
name: Restore Maven Cache

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Restore Maven cache from GCS
        uses: Unifits/unifits-github-actions/maven-cache-restore@v1
        with:
          gcs-bucket: 'my-maven-cache-bucket'
```

## Notes

- Uses the `danySam/gcs-cache/restore` action under the hood for GCS operations.
- The cache key should be coordinated with your cache save step for best results.
- Works best when paired with a corresponding cache save action after the build.
