# 🔄 Change Data Capture (CDC)

Change Data Capture (CDC) is a technique for identifying and capturing changes made to data in a database and delivering those changes in real time or near real time to downstream systems.

---

## 📌 Why CDC?

CDC enables:

- **Real-time data replication** to warehouses, lakes, or caches
- **Reduced load** compared to full table scans
- **Auditability** and change tracking
- **Incremental ETL** processes for efficient pipelines

---

## 🧰 Common CDC Methods

### 1. **Log-Based CDC**
Reads changes from the database’s transaction log (WAL, binlog, redo log).

✅ Pros:
- Minimal performance impact
- Captures all changes, including deletes

🚫 Cons:
- Requires access to DB internals
- More setup complexity

🔧 Examples:
- MySQL binlog
- PostgreSQL WAL
- Debezium

---

### 2. **Trigger-Based CDC**
Uses database triggers to write changes into shadow/audit tables.

✅ Pros:
- Flexible logic (can enrich or filter)

🚫 Cons:
- Adds write latency
- Can be harder to maintain

---

### 3. **Timestamp-Based CDC**
Queries data where `updated_at` or `modified_at` > last sync time.

✅ Pros:
- Easy to implement

🚫 Cons:
- Misses deletes
- Depends on consistent timestamp updates

---

## 🔄 Typical CDC Workflow

```mermaid
graph TD
  A[Source DB] --> B[CDC Agent (Debezium, Fivetran)]
  B --> C[Kafka / Stream]
  C --> D[Sink (Warehouse, Data Lake)]
```

---

## 💡 Use Cases

- Syncing OLTP systems to data warehouses
- Feeding event-driven microservices
- Updating machine learning features incrementally
- Building audit trails

---

## ⚠️ Challenges

- Handling schema evolution
- Ensuring data ordering and consistency
- Backfill + reprocessing logic
- Latency vs cost tradeoffs

---

## ✅ Best Practices

- Use **log-based CDC** when possible for completeness
- Track **schema versions** and store metadata
- Apply **idempotent transformations** downstream
- Combine with **watermarking** or **checkpoints**

---

## 📚 References

- [Debezium Documentation](https://debezium.io/documentation/)
- [Fivetran CDC Overview](https://fivetran.com/docs/concepts/cdc)
- [PostgreSQL Logical Replication](https://www.postgresql.org/docs/current/logical-replication.html)
- [Kafka Connect CDC](https://www.confluent.io/hub/debezium/)

---

🔁 CDC turns your database into a stream of truth!
