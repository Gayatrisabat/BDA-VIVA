# 6. Hadoop Ecosystem

The Hadoop ecosystem is a collection of tools for storing, processing, accessing, transferring, and managing large datasets.

## 1. Data storage layer

### HDFS
Distributed file storage using blocks and DataNodes.

### HBase
A distributed, column-family-oriented NoSQL database for large tables and low-latency random reads/writes.

## 2. Data processing layer

### MapReduce
Processes data using Map, shuffle/sort, and Reduce.

### YARN
Manages cluster resources and schedules applications.

## 3. Data access layer

| Tool | Purpose | Key term |
|---|---|---|
| Hive | Data warehousing and SQL-like querying | HiveQL |
| Pig | Data processing with scripts | Pig Latin |
| Mahout | Scalable machine-learning algorithms | Clustering/classification |
| Avro | Schema-based data serialization | Serialization |
| Sqoop | Bulk transfer between relational databases and Hadoop | Import/export |

### Hive vs. Pig
Hive is oriented toward SQL-like querying and data warehousing. Pig uses procedural data-flow scripts for transformations.

## 4. Data management layer

| Tool | Purpose |
|---|---|
| Oozie | Workflow scheduling |
| Chukwa | Distributed log collection and monitoring |
| Flume | Collection and movement of event/log data |
| ZooKeeper | Coordination of distributed services |

## Rapid-fire questions
- Distributed file storage? **HDFS**
- Cluster resources? **YARN**
- Map and Reduce processing? **MapReduce**
- SQL-like queries? **Hive**
- Pig scripting language? **Pig Latin**
- NoSQL random access? **HBase**
- Bulk RDBMS transfer? **Sqoop**
- Workflow scheduler? **Oozie**
- Log ingestion? **Flume**
- Distributed coordination? **ZooKeeper**
- Schema-based serialization? **Avro**
- Scalable ML algorithms? **Mahout**

**Note:** Some tools in this syllabus are legacy or less common in modern platforms. Learn their course definitions for the viva.
