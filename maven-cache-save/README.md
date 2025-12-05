# Maven Cache Save (GCS) GitHub Action

This GitHub Action prunes SNAPSHOT artifacts from the local Maven repository and saves the cache to Google Cloud Storage (GCS). It is designed to optimize Maven build caching in CI/CD pipelines by storing only release artifacts, reducing cache size and improving restore performance.

## Features

- **Prune SNAPSHOT Artifacts**: Removes SNAPSHOT dependencies from the local Maven repository before caching.
- **Save to GCS**: Uploads the pruned Maven repository to a specified GCS bucket.
- **Customizable Patterns and Paths**: Supports custom prune patterns, cache keys, GCS bucket paths, and local repository locations.
- **Optimized for CI/CD**: Ideal for workflows running on GitHub Actions with Google Cloud integration.

## Inputs

| Name             | Description                                                                 | Required | Default                                               |
|------------------|-----------------------------------------------------------------------------|----------|-------------------------------------------------------|
| `cache-key`      | Cache key matching the restore step to ensure consistency                   | No       | `maven-${{ runner.os }}-${{ github.repository }}-${{ hashFiles('**/pom.xml') }}` |
| `gcs-bucket`     | Name of the GCS bucket to store the cache                                  | Yes      | -                                                     |
| `gcs-path-prefix`| Path prefix within the bucket where the cache will be saved                | No       | `maven/${{ github.repository }}/releases`             |
| `path`           | Local path to the Maven repository to save                                 | No       | `~/.m2/repository`                                    |
| `prune-pattern`  | Pattern used to exclude jars                                               | No       | `*-SNAPSHOT*`                                         |

## Example Workflow

```yaml
name: Save Maven Cache

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Save Maven cache to GCS
        uses: Unifits/unifits-github-actions/maven-cache-save@v1
        with:
          gcs-bucket: 'my-maven-cache-bucket'
```

## Notes

- Uses the `danySam/gcs-cache/save` action under the hood for GCS operations.
- The prune pattern can be customized to exclude other types of artifacts if needed.
- Works best when paired with a corresponding cache restore action at the start of your workflow.
