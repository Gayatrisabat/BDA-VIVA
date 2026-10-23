# 8. Viva Questions and Answers

## Big Data basics
1. **What is Big Data?** Large, complex, rapidly generated datasets that may require specialised technologies to store and process efficiently.
2. **What are the 5 Vs?** Volume, Velocity, Variety, Veracity, Value.
3. **What is Volume?** Amount of data.
4. **What is Velocity?** Speed of data generation and processing.
5. **What is Variety?** Different data types and formats.
6. **What is Veracity?** Quality and trustworthiness.
7. **What is Value?** Useful insights obtained from data.
8. **What is data mining?** Discovering patterns and relationships in data.
9. **What is structured data?** Data in a predefined schema, commonly rows and columns.
10. **What is unstructured data?** Data without a fixed tabular schema, such as images and videos.
11. **What is semi-structured data?** Data with keys, tags, or metadata, such as JSON and XML.
12. **What is horizontal scaling?** Adding machines to increase capacity.
13. **What is vertical scaling?** Increasing resources on an existing machine.

## Hadoop basics
14. **What is Hadoop?** An open-source framework for distributed storage and processing.
15. **What is HDFS?** Hadoop Distributed File System, Hadoop's distributed storage component.
16. **What is MapReduce?** A programming model/framework for distributed computation.
17. **What is YARN?** Yet Another Resource Negotiator; manages cluster resources and scheduling.
18. **What is Hadoop Common?** Shared libraries and utilities.
19. **What is a node?** An individual machine participating in a cluster.
20. **What is a cluster?** A group of interconnected machines working together.
21. **What is the NameNode?** HDFS metadata and namespace manager.
22. **What is the DataNode?** Service that stores actual HDFS blocks.
23. **What is a heartbeat?** Periodic message showing a DataNode is alive.
24. **What is replication?** Maintaining multiple copies of data blocks.
25. **What is fault tolerance?** Ability to handle failures and recover appropriately.
26. **What is data locality?** Processing data near where it is stored.

## Ecosystem
27. **What is Hive?** A data-warehouse tool supporting SQL-like queries via HiveQL.
28. **What is Pig?** A data-processing platform using Pig Latin.
29. **What is HBase?** A distributed NoSQL database for large-scale random access.
30. **What is Sqoop?** A bulk-transfer tool between relational databases and Hadoop.
31. **What is Flume?** A distributed log/event collection tool.
32. **What is Oozie?** A workflow scheduler.
33. **What is ZooKeeper?** A coordination service for distributed systems.
34. **What is Avro?** A schema-based serialization framework.
35. **What is Mahout?** A scalable machine-learning library.
36. **What is Chukwa?** A distributed log collection and monitoring system.

## Algorithms and numericals
37. **What are the main MapReduce stages?** Map, shuffle/sort, and Reduce.
38. **What does a mapper emit?** Intermediate key-value pairs.
39. **What does a reducer do?** Aggregates values associated with the same key.
40. **How many 128 MB blocks are needed for 500 MB?** Four.
41. **What storage is needed for 500 MB with replication factor 3?** Approximately 1500 MB, excluding overhead.
42. **What is word count for `cat dog cat`?** `cat → 2`, `dog → 1`.
43. **Why is MapReduce less suitable for some real-time jobs?** It is primarily batch-oriented and may introduce high latency.
44. **What is the small-files problem?** Many small files create NameNode metadata and processing overhead.
45. **Name a stream-ingestion and processing technology.** Kafka for ingestion; Spark for processing.
