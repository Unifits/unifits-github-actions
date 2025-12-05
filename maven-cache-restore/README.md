
# Maven Cache Restore (GCS)

**Description:**  
Restores the Maven local repository cache from Google Cloud Storage to speed up builds and reduce dependency download time.  
This composite GitHub Action retrieves a Maven cache from a Google Cloud Storage (GCS) bucket and restores it to the local Maven repository directory (`~/.m2/repository` by default).

---

## 🚀 Features

- Restores Maven dependencies from a remote GCS bucket.
- Supports custom cache keys and path prefixes for flexible caching strategies.
- Speeds up Maven builds by avoiding repeated downloads of dependencies.

---

## 📥 Inputs

| Name            | Description                                                                                     | Required | Default                                                                                                   |
|-----------------|-------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------|
| `cache-key`     | Primary cache key for the Maven repository (e.g., `maven-<os>-<repo>-<hash>`).                | No       | `maven-${{ runner.os }}-${{ github.repository }}-${{ hashFiles('**/pom.xml') }}`                        |
| `gcs-bucket`    | Name of the GCS bucket that stores the cache.                                                 | **Yes**  | —                                                                                                         |
| `gcs-path-prefix`| Path prefix within the bucket (e.g., `maven/<repo>/releases`).                               | No       | `maven/${{ github.repository }}/releases`                                                                |
| `path`          | Local path to the Maven repository.                                                           | No       | `~/.m2/repository`                                                                                       |

---

## 📤 Outputs

| Name                | Description                                                                 |
|---------------------|-----------------------------------------------------------------------------|
| `cache-primary-key` | The primary key used for the restored cache; useful for subsequent save step.|

---

## 📝 Usage Example

```yaml
- name: Restore Maven cache from GCS
  id: cache-restore
  uses: ./.github/actions/maven-cache-restore
  with:
    gcs-bucket: unifits-euw-arc-cache-prd-build
    # Optional overrides:
    # cache-key: "maven-${{ runner.os }}-${{ github.repository }}-${{ hashFiles('**/pom.xml') }}"
    # gcs-path-prefix: "maven/${{ github.repository }}/releases"
    # path: "~/.m2/repository"

- name: Print restored cache key
  run: echo "Cache key: ${{ steps.cache-restore.outputs.cache-primary-key }}"
