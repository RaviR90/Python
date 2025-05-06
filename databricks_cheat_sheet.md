
# 📘 Databricks Cheat Sheet

---

## 🔧 Basic Notebook Commands

| Shortcut / Syntax        | Description                                |
|--------------------------|--------------------------------------------|
| `%python`, `%sql`, `%scala`, `%r` | Set cell language                   |
| `dbutils.fs.ls("/mnt/...")`       | List files in mounted storage       |
| `dbutils.widgets.text("input", "", "Label")` | Create a text input widget |
| `dbutils.notebook.run("notebook_name", timeout, {"param": "value"})` | Call another notebook |

---

## 💾 DataFrame Basics (PySpark)

```python
df = spark.read.csv("/mnt/data.csv", header=True, inferSchema=True)
df.show()
df.printSchema()
df.select("col1", "col2").filter(df["col3"] > 100).display()
df.write.mode("overwrite").parquet("/mnt/output")
```

---

## 📊 SQL in Notebooks

```sql
%sql
SELECT * FROM table_name WHERE column > 100
CREATE TABLE my_table USING delta LOCATION '/mnt/mydata'
```

---

## 🧠 MLflow Tracking (Auto-logging)

```python
import mlflow
mlflow.autolog()
with mlflow.start_run():
    model = train_model()
    mlflow.sklearn.log_model(model, "model")
```

---

## 🔁 Delta Lake Commands

```python
# Read
df = spark.read.format("delta").load("/mnt/delta_table")

# Write
df.write.format("delta").mode("overwrite").save("/mnt/delta_table")

# SQL
%sql
CREATE TABLE my_delta_table USING delta LOCATION '/mnt/delta_table'
```

---

## ⚙️ Job & Workflow Management

| Tool / API                    | Use                                             |
|------------------------------|--------------------------------------------------|
| Jobs UI                      | Schedule notebook pipelines                      |
| `dbutils.notebook.run(...)`  | Trigger notebooks from other notebooks           |
| Workflows (multi-task jobs)  | Manage dependencies and conditional logic        |

---

## 🔥 Cluster & Runtime Tips

- Use **cluster pools** to reduce startup time
- Prefer **auto-scaling clusters** for variable workloads
- Use **Databricks runtime with ML** for pre-installed libraries

---

## 🛠️ Common Utilities

| Command                         | Description                          |
|----------------------------------|--------------------------------------|
| `dbutils.fs.mount(...)`          | Mount external storage               |
| `dbutils.secrets.get(...)`       | Retrieve secrets from Key Vault      |
| `display(df)`                    | Render table/graph from DataFrame    |
| `spark.sql("...")`               | Run SQL using Spark API              |

---

## 📁 Mounting External Storage (Blob/S3)

```python
configs = {"fs.azure.account.key.<storage>.blob.core.windows.net": "<key>"}
dbutils.fs.mount(source = "wasbs://<container>@<storage>.blob.core.windows.net",
                 mount_point = "/mnt/mydata",
                 extra_configs = configs)
```

---

## 📚 File Paths

| Example Path                                | Meaning                             |
|---------------------------------------------|-------------------------------------|
| `/dbfs/mnt/mydata`                          | Mounted blob storage path           |
| `dbfs:/mnt/mydata`                          | Same as above (URI format)          |
| `/tmp/` or `/dbfs/tmp/`                     | Temp space within the cluster       |

---

## ✅ Best Practices

- Use **Delta Lake** for all data writes (versioning, rollback, schema enforcement)
- Version control notebooks using **Repos (Git Integration)**
- Use **parameterized notebooks** with widgets
- Avoid writing directly to `/databricks/driver/` — use `/dbfs/` or mount points
