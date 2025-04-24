### **A quick tour of Presto’s architecture**

Think of Presto as a middle layer that lets you run one SQL query over data that sits in many different places. A typical cluster has two kinds of processes:

| Component | What it does |
| :---- | :---- |
| **Coordinator** | Receives every SQL query, checks security, figures out the best execution plan, and then breaks the job into lots of tiny tasks. |
| **Workers** | Run those tasks in parallel, reading data directly from the underlying storage systems (S3, Hive, MySQL, Kafka, etc.), shuffling partial results among themselves, and sending the final answer back to the coordinator. |

Under the hood, the workflow looks like this:

1. **Parsing & planning**  
    The coordinator parses the SQL, builds a logical plan (scans, joins, filters, aggregates), and then turns it into a physical plan that decides join types, data partitioning, and degrees of parallelism.

2. **Task scheduling**  
    That plan is split into stages. Each stage contains many tasks, and each task is a pipeline of operators that runs on one worker thread. The coordinator places tasks on workers, trying to keep them close to the data and balancing the load across the cluster.

3. **In-memory, pipelined execution**  
    Workers process data in small, columnar pages that stay in RAM as long as possible. Operators are pipelined, so rows flow straight from a scan to a filter to a join without ever touching disk unless memory pressure forces a spill.

4. **Connectors**  
    Every data source is accessed through a connector—a small plugin that knows how to list tables, split data into chunks, and push down predicates. Because all connectors implement the same interface, a query that joins a Kafka topic to a MySQL table “just works.”

5. **Result return & cleanup**  
    As soon as the final stage finishes, the coordinator streams the result set back to the client. Tasks are garbage-collected, and any spilled files are deleted.

Key takeaways:

* **No data movement up front** – Presto reads data where it lives; you don’t load it into a separate warehouse first.

* **Pure compute layer** – The cluster is stateless and elastic: add or remove workers without touching the data.

* **Interactive speed** – By keeping intermediates in memory and parallelizing aggressively, most analytic queries finish in seconds.

* **Single SQL dialect** – Regardless of whether data is in Parquet on S3 or rows in PostgreSQL, you use the same ANSI SQL.

That’s the essence of Presto: one coordinator, many workers, a connector for every data source, and fast, in-memory execution that stitches everything together.

