# 4. Hadoop Architecture

## Overview
A Hadoop cluster contains services for storage, resource management, and processing. Different services may run on different machines.

### Main roles
- **NameNode:** Manages HDFS filesystem metadata.
- **DataNode:** Stores HDFS data blocks.
- **ResourceManager:** Manages cluster resources through YARN.
- **NodeManager:** Manages resources and containers on an individual worker node.
- **ApplicationMaster:** Coordinates an individual YARN application.
- **MapReduce tasks:** Perform map and reduce computation.

## Simplified workflow
1. A user submits a job.
2. Input data is stored in HDFS.
3. The NameNode provides metadata and block locations.
4. YARN allocates resources to the application.
5. Tasks process data partitions across the cluster.
6. Intermediate results are grouped and reduced.
7. The output is written to the configured destination, often HDFS.

## Master/control roles vs. worker roles

| Control service | Responsibility |
|---|---|
| NameNode | HDFS namespace and block metadata |
| ResourceManager | Cluster resource allocation and scheduling |

| Worker service | Responsibility |
|---|---|
| DataNode | Stores and serves HDFS blocks |
| NodeManager | Manages containers and resources on a worker |

**Important:** The NameNode and ResourceManager are different services with different responsibilities. They do not have to be on the same physical machine.

## Why is data locality useful?
It reduces unnecessary network transfers by scheduling computation near the data, improving efficiency for suitable workloads.
