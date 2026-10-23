# 5. Hadoop Distributed File System (HDFS)

## What is HDFS?
HDFS stands for Hadoop Distributed File System. It stores large files by dividing them into blocks and distributing the blocks across DataNodes.

It is designed for high-throughput access to large files and uses replication to help tolerate failures.

## Components

### NameNode
- Manages filesystem metadata and namespace.
- Tracks file and directory information.
- Tracks which DataNodes hold each block.
- Manages filesystem permissions.
- Does not ordinarily store the actual file contents as HDFS blocks.

### DataNode
- Stores actual HDFS blocks.
- Serves client read and write requests.
- Sends heartbeats and block reports.
- Participates in replication and recovery.

### Client
- Requests file operations.
- Contacts the NameNode for metadata and block locations.
- Transfers file data directly to/from DataNodes.

## HDFS write workflow
1. Client requests file creation.
2. NameNode checks namespace rules and permissions.
3. NameNode identifies suitable DataNodes for blocks and replicas.
4. Client streams data to a DataNode pipeline.
5. DataNodes store and forward blocks to the next DataNode in the pipeline.
6. Acknowledgements return through the pipeline.
7. The write is completed and metadata is maintained.

## HDFS read workflow
1. Client requests a file.
2. NameNode returns block locations.
3. Client reads blocks directly from suitable DataNodes.
4. Client assembles the blocks in the correct order.

## Replication
Replication means keeping multiple copies of a data block on different DataNodes.

A replication factor of 3 means HDFS aims to maintain three replicas per block, subject to successful placement and cluster conditions.

## Common defaults
- Block size: 128 MB in many modern configurations.
- Replication factor: 3 in many configurations.

Both are configurable and are not universal fixed values.

## HDFS vs. HBase
- **HDFS:** Distributed file storage, optimised for large files and throughput.
- **HBase:** Distributed NoSQL database providing database-style random access to records, commonly using HDFS underneath.

## Common viva questions
**What is a heartbeat?** A periodic message indicating that a DataNode is alive.

**What is a block report?** A report listing blocks stored by a DataNode.

**What if a DataNode fails?** The NameNode detects missing heartbeats; clients may read another replica, and HDFS can arrange re-replication.

**What if a NameNode fails?** Filesystem operations can be disrupted. High-availability deployments can use active and standby NameNodes.
