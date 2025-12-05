# Maven Cache Save (GCS) GitHub Action

# Maven Cache Save (GCS)

**Description:**  
Prunes SNAPSHOT artifacts from the local Maven repository and saves the cache to Google Cloud Storage.  
This composite GitHub Action helps optimize Maven builds by removing unnecessary SNAPSHOT dependencies and uploading the cleaned repository to a remote GCS bucket for future reuse.

- **Prune SNAPSHOT Artifacts**: Removes SNAPSHOT dependencies from the local Maven repository before caching.
- **Save to GCS**: Uploads the pruned Maven repository to a specified GCS bucket.
- **Customizable Patterns and Paths**: Supports custom prune patterns, cache keys, GCS bucket paths, and local repository locations.
- **Optimized for CI/CD**: Ideal for workflows running on GitHub Actions with Google Cloud integration.

## Inputs

- Deletes all SNAPSHOT artifacts from the local Maven repository.
- Saves the cleaned Maven cache to a specified GCS bucket and path prefix.
- Supports custom cache keys and repository paths for flexible caching strategies.

---

## 📥 Inputs

| Name            | Description                                                                                     | Required | Default                                                                                                   |
|-----------------|-------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------|
| `cache-key`     | Cache key matching the restore step to ensure consistency.                                      | No       | `maven-${{ runner.os }}-${{ github.repository }}-${{ hashFiles('**/pom.xml') }}`                        |
| `gcs-bucket`    | Name of the GCS bucket to store the cache.                                                     | Yes      | —                                                                                                         |
| `gcs-path-prefix`| Path prefix within the bucket where the cache will be saved.                                  | No       | `maven/${{ github.repository }}/releases`                                                                |
| `path`          | Local path to the Maven repository to save.                                                    | No       | `~/.m2/repository`                                                                                       |
| `prune-pattern` | Pattern used to delete SNAPSHOT directories.                                                   | No       | `*-SNAPSHOT*`                                                                                            |

---

## 📤 Outputs

This action does not define explicit outputs.

---

## 📝 Usage Example

```yaml
- name: Save Maven cache to GCS (releases only)
  uses: ./.github/actions/maven-cache-save
  with:
    gcs-bucket: unifits-euw-arc-cache-prd-build
    # Optional overrides:
    # cache-key: "maven-${{ runner.os }}-${{ github.repository }}-${{ hashFiles('**/pom.xml') }}"
    # gcs-path-prefix: "maven/${{ github.repository }}/releases"
    # path: "~/.m2/repository"
    # prune-pattern: "*-SNAPSHOT*"
``
