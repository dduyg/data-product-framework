# 🧾 Data Versioning

Data versioning is the practice of keeping track of changes to datasets over time, enabling reproducibility, auditing, and rollback capabilities.

## 📌 Why Version Data?
Just like code, data evolves. Versioning helps teams:
- **Reproduce past results** reliably
- **Audit** what data was used when
- **Compare model performance** across dataset snapshots
- **Rollback** to previous dataset states

---

## 🧰 Tools & Approaches

### 🔹 1. File-Based Versioning
Use source control (like Git or DVC) to manage dataset versions:

- **DVC (Data Version Control):**
  - Works with Git
  - Tracks data files, pipelines
  - Stores large files in remote storage (S3, GCS)

- **LakeFS:**
  - Git-like interface for object stores
  - Branching, merging for data lakes

### 🔹 2. Table-Based Versioning
For structured datasets in warehouses/lakes:

- **Delta Lake** (Databricks):
  - Time travel with `VERSION AS OF`
- **Apache Hudi / Iceberg**:
  - Store commit metadata
  - Enable rollback and incremental queries

```sql
-- Example: Delta Lake time travel
SELECT * FROM my_table VERSION AS OF 42;
```

### 🔹 3. Metadata + Hashing
For lightweight use cases:

- Use hashing (e.g. MD5) of dataset content
- Store metadata and hashes in an audit table
- Trigger updates only if content changes

---

## 🧠 Versioning Strategies

| Strategy | Best For |
| --- | --- |
| Snapshot Tables | Periodic full copies (e.g. monthly) |
| Append + Partitioning | Immutable event data |
| Soft Deletes + History Tables | Changing dimension data |

---

## 🔍 Common Use Cases

- ML model training and validation reproducibility
- Debugging unexpected behavior in production
- Audit compliance (e.g. GDPR, SOX)

---

## ⚠️ Challenges

- Versioning high-volume data efficiently
- Managing cost of snapshots and duplicates
- Ensuring downstream dependencies are aware of changes

---

## ✅ Best Practices

- Track schema as well as data content
- Log data ingestion timestamps and source versions
- Automate versioning with CI/CD-style tooling
- Use consistent partitioning and naming schemes

---

## 📚 References

- [DVC Docs](https://dvc.org/doc)
- [Delta Lake — Time Travel](https://docs.delta.io/latest/delta-utility.html#time-travel)
- [LakeFS Overview](https://lakefs.io/)
- [Data Version Control with Git](https://towardsdatascience.com/data-version-control-101-7e00bafed7ac)

> 📦 Version your data, just like your code!
>
