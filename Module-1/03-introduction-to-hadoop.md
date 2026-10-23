# 3. Introduction to Hadoop

## What is Hadoop?
Apache Hadoop is an open-source framework for distributed storage and processing of large datasets across a cluster of computers.

Instead of relying on one machine, Hadoop distributes data and computation across multiple nodes.

## Why do we need Hadoop?
- A single machine may have limited storage and processing capacity.
- Large sequential workloads can take too long.
- Scaling a single powerful machine can be expensive.
- Data may arrive in different formats.
- Distributed systems need mechanisms to handle node failures.

## Features
- Open-source software
- Distributed storage
- Parallel processing
- Scale-out architecture
- Replication-based fault tolerance
- Support for many data formats
- High-throughput processing of large datasets

## Core components of Hadoop

| Component | Full name / role |
|---|---|
| Hadoop Common | Shared libraries and utilities |
| HDFS | Hadoop Distributed File System; distributed storage |
| MapReduce | Distributed processing model/framework |
| YARN | Yet Another Resource Negotiator; cluster resource management and scheduling |

**Memory aid:** Common = support; HDFS = storage; MapReduce = processing; YARN = resource management.

## Hadoop advantages
- Handles large datasets
- Can process data in parallel
- Can scale by adding nodes
- Can use commodity hardware
- Provides replication and recovery mechanisms

## Hadoop limitations
- Traditional MapReduce is primarily batch-oriented.
- Large numbers of small files can cause HDFS metadata overhead.
- Cluster operation and tuning can be complex.
- Iterative machine-learning tasks may be inefficient in MapReduce.
- Real-time workloads often need additional streaming technologies.

## Important terms
- **Node:** An individual machine in a cluster.
- **Cluster:** A collection of machines working together.
- **Horizontal scaling:** Adding more machines.
- **Vertical scaling:** Increasing resources on an existing machine.
- **Fault tolerance:** Ability to handle failures and recover.
- **Data locality:** Moving computation close to the data.
