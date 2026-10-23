# External Viva Preparation: Relational Operations using MapReduce, CAP Theory, and NoSQL

## Basic to Advanced Questions and Answers

**Prepared from the three supplied PDFs:** 1. *Relational Operations
using map reduce - bda.pdf* (6 scanned pages) 2. *CAP Theory - BDA.pdf*
(4 pages) 3. *Module_3_NOSQL.pdf* (24 pages)

**Study note:** The guide preserves the terminology and examples used in
the notes. Where a scanned handwritten table is difficult to read, the
algorithm is explained using the legible values and the same method. Any
brief clarification is identified as such.

------------------------------------------------------------------------

# PART A --- RELATIONAL OPERATIONS USING MAPREDUCE

## A1. Fundamentals

### Q1. What is relational algebra?

**Answer:** Relational algebra is a collection of operations used to
retrieve, filter, combine, and summarize data stored in relations
(tables). The operations in the notes are Union, Difference, Natural
Join, and Grouping & Aggregation. Related operations commonly discussed
with MapReduce include Selection and Projection.

### Q2. What is a relation?

**Answer:** A relation is a table consisting of rows and columns. Each
row is a tuple (record), and each column represents an attribute.

### Q3. What is a tuple?

**Answer:** A tuple is one row in a relation. For example, `(1, 2)` is a
tuple with attribute values A = 1 and B = 2.

### Q4. What is MapReduce?

**Answer:** MapReduce is a distributed processing model that processes
input records in parallel. The Map phase emits intermediate key-value
pairs, Shuffle and Sort groups values by key, and the Reduce phase
combines or processes those grouped values to produce output.

### Q5. Why use MapReduce for relational operations?

**Answer:** MapReduce can process large relations distributed across
multiple workers. Mappers process records in parallel, and Reducers
combine records with the same key to perform operations such as
duplicate elimination, set operations, joins, and aggregation.

### Q6. What is a key-value pair in these algorithms?

**Answer:** A key determines which records are grouped together. The
value carries the tuple or extra information needed by the Reducer. For
example, for a set operation, the tuple itself can be the key; for a
join, the common join attribute is the key.

### Q7. What is Shuffle and Sort?

**Answer:** Shuffle transfers intermediate records to the correct
Reducers. Sort/grouping places records with the same key together so one
Reducer can process the complete group for that key.

### Q8. What is a Map worker?

**Answer:** A Map worker executes the Map function on its assigned input
records or table partitions and emits intermediate key-value pairs.

### Q9. What is a Reduce worker?

**Answer:** A Reduce worker receives grouped intermediate records and
applies the operation's logic---for example, removing duplicates,
checking tuple origin, joining records, or calculating an aggregate.

------------------------------------------------------------------------

## A2. Union

### Q10. What is Union?

**Answer:** Union combines the tuples from two relations, Table 1 and
Table 2. In the relational set model, duplicate tuples appear only once
in the result.

**Notation:** (R `\cup `{=tex}S)

### Q11. What condition is required for Union?

**Answer:** The relations should be union-compatible: they must have
compatible attributes and corresponding data types so their tuples can
be combined.

### Q12. How is Union implemented using MapReduce?

**Answer:** 1. The Mapper reads a tuple from either relation. 2. It
emits the tuple as both key and value, such as `((1,2), (1,2))`. 3.
Shuffle and Sort groups identical tuples under the same key. 4. The
Reducer emits one copy of each key, eliminating duplicates.

### Q13. Explain the Union example from the notes.

The tables shown in the handwritten notes contain these tuples:

**Table 1**

``` text
(1,2)
(3,1)
```

**Table 2**

``` text
(1,2)
(2,1)
```

The tuple `(1,2)` appears in both relations.

**Map output (conceptual):**

``` text
Key (1,2) -> values [(1,2), (1,2)]
Key (3,1) -> values [(3,1)]
Key (2,1) -> values [(2,1)]
```

**Reduce rule:** Emit one tuple for each distinct key.

**Union result:**

``` text
(1,2)
(3,1)
(2,1)
```

### Q14. Why does Union need grouping?

**Answer:** The same tuple may appear in both input relations or more
than once. Grouping identical tuple keys allows the Reducer to emit a
single copy.

### Q15. Is Union the same as concatenating two tables?

**Answer:** No. Concatenation would simply place all rows together and
may retain duplicates. Relational Union in the set model removes
duplicates.

------------------------------------------------------------------------

## A3. Difference

### Q16. What is Difference?

**Answer:** Difference (R-S) returns tuples that are present in relation
R but absent from relation S.

### Q17. How is Difference implemented using MapReduce?

**Answer:** 1. For each tuple from Table 1/R, the Mapper emits
`(tuple, T1)`. 2. For each tuple from Table 2/S, the Mapper emits
`(tuple, T2)`. 3. Shuffle and Sort groups records by tuple. 4. The
Reducer examines the source tags. 5. It emits a tuple only if it occurs
in T1/R and does not occur in T2/S.

### Q18. Why do we use T1 and T2 labels?

**Answer:** The labels identify which input relation a tuple came from.
Without source labels, the Reducer could not reliably determine whether
a tuple belongs only to R or also appears in S.

### Q19. Explain the Difference example from the notes.

The input tables shown are:

**Table 1 / T1**

``` text
(1,2)
(3,1)
```

**Table 2 / T2**

``` text
(1,2)
(2,1)
```

The tuple `(1,2)` is present in both tables, so it is excluded from
(T1-T2). The tuple `(3,1)` appears only in T1, so it remains. The tuple
`(2,1)` appears only in T2, so it is not included.

**Result (T1-T2):**

``` text
(3,1)
```

### Q20. What if a tuple appears in T2 but not in T1?

**Answer:** It is not included in (T1-T2), because Difference is
directional. It returns records in the first relation that are absent
from the second.

### Q21. Is (R-S) the same as (S-R)?

**Answer:** No. Difference is not commutative. The results can differ
because the first relation defines which tuples are candidates for
inclusion.

### Q22. What is the difference between Union and Difference?

**Answer:** Union returns tuples from either relation, with duplicates
removed. Difference returns only tuples that occur in the first relation
and not in the second.

------------------------------------------------------------------------

## A4. Natural Join

### Q23. What is Natural Join?

**Answer:** Natural Join combines tuples from two relations using
attributes with the same name. It matches rows where the common
attribute values are equal and combines their remaining attributes.

### Q24. What is the join key in the notes' example?

**Answer:** The relations are `Table 1(A,B)` and `Table 2(B,C)`. The
common attribute is **B**, so B is used as the join key.

### Q25. How is Natural Join implemented using MapReduce?

**Answer:** 1. The Mapper reads a tuple from either relation. 2. It
emits the common attribute value as the key. 3. The value includes a
relation tag and the remaining tuple values, for example `(T1, A)` or
`(T2, C)`. 4. Shuffle and Sort groups records with the same B value. 5.
The Reducer separates records from T1 and T2. 6. It combines each
compatible T1 record with each T2 record that has the same B value.

### Q26. Why are relation tags needed in a join?

**Answer:** Tags such as T1 and T2 tell the Reducer which relation each
record belongs to. The Reducer needs records from both relations to form
joined tuples.

### Q27. Explain the Natural Join example from the notes.

The notes show:

**Table 1(A,B)**

``` text
A  B
1  2
2  3
5  6
```

**Table 2(B,C)**

``` text
B  C
2  3
4  4
6  1
```

Join on common attribute B.

-   For `B = 2`, Table 1 has `(A=1,B=2)` and Table 2 has `(B=2,C=3)`.
    Output: `(A=1,B=2,C=3)`.
-   For `B = 3`, Table 1 has `(A=2,B=3)`, but Table 2 has no row with
    `B=3` in this first table version. No match is produced.
-   For `B = 6`, Table 1 has `(A=5,B=6)` and Table 2 has `(B=6,C=1)`.
    Output: `(A=5,B=6,C=1)`.

**Join results from these rows:**

``` text
A  B  C
1  2  3
5  6  1
```

*Exam note:* Later rows in the scanned pages show additional sample
values. The core method is to group by B and combine only matching T1/T2
records.

### Q28. What happens if a join key exists in only one relation?

**Answer:** No joined tuple is produced for that key because a Natural
Join requires a matching record from both relations.

### Q29. What happens if multiple rows from each relation have the same join key?

**Answer:** The Reducer combines every compatible row from one relation
with every compatible row from the other relation for that key. This can
produce multiple output tuples.

### Q30. Differentiate Union and Natural Join.

**Answer:** Union combines rows from compatible relations and removes
duplicates. Natural Join matches rows on common attributes and combines
their columns.

------------------------------------------------------------------------

## A5. Grouping and Aggregation

### Q31. What is Grouping?

**Answer:** Grouping collects records that have the same values for one
or more selected attributes.

### Q32. What is Aggregation?

**Answer:** Aggregation calculates a summary value over each group.
Examples include `MAX`, `MIN`, `SUM`, `COUNT`, and `AVG`.

### Q33. What grouping and aggregation task is specified in the notes?

**Answer:** The notes ask to **group by (A, B) and apply MAX as the
aggregation on C**.

### Q34. How is Grouping and Aggregation implemented using MapReduce?

**Answer:** 1. The Mapper reads each tuple `(A,B,C,D)`. 2. It emits the
grouping attributes `(A,B)` as the key. 3. It emits C as the value
because MAX is being calculated on C. 4. Shuffle and Sort groups all C
values by `(A,B)`. 5. The Reducer calculates the maximum C value for
each key. 6. The output contains `(A,B,MAX(C))`.

### Q35. Why is `(A,B)` the key?

**Answer:** The task requires records with the same A and B values to be
grouped together. Using `(A,B)` as the key sends all records for a group
to the same Reducer.

### Q36. Why is C emitted as the value?

**Answer:** C is the attribute on which the maximum is calculated. The
Reducer needs all C values associated with each `(A,B)` group.

### Q37. Work a simple MAX grouping example.

Input:

``` text
A  B  C
1  2  10
1  2   9
2  3   2
2  3   5
4  2   7
```

Mapper output:

``` text
(1,2) -> 10
(1,2) -> 9
(2,3) -> 2
(2,3) -> 5
(4,2) -> 7
```

After grouping:

``` text
(1,2) -> [10,9]
(2,3) -> [2,5]
(4,2) -> [7]
```

Reducer calculates MAX:

``` text
A  B  MAX(C)
1  2    10
2  3     5
4  2     7
```

### Q38. Can the same pattern be used for SUM or COUNT?

**Answer:** Yes. Keep `(A,B)` as the key, emit the appropriate value,
and replace the Reducer operation with SUM or COUNT. For AVG, the
Reducer can compute sum and count and then divide.

### Q39. What is the difference between Grouping and Aggregation?

**Answer:** Grouping identifies which records belong together.
Aggregation calculates a summary from the values in each group.

### Q40. What is the role of Shuffle and Sort in grouping?

**Answer:** It collects all records with the same `(A,B)` key together,
so the Reducer can see every C value required to calculate the maximum.

------------------------------------------------------------------------

## A6. Algorithm and Viva Follow-ups

### Q41. Write pseudocode for Union.

``` text
MAP(relation, tuple t):
    emit(t, t)

REDUCE(tuple t, values):
    emit(t)       // emit once per distinct key
```

### Q42. Write pseudocode for Difference (R-S).

``` text
MAP(source, tuple t):
    if source is R:
        emit(t, "R")
    else if source is S:
        emit(t, "S")

REDUCE(tuple t, source_tags):
    if "R" is present and "S" is absent:
        emit(t)
```

### Q43. Write pseudocode for Natural Join.

``` text
MAP(source, tuple t):
    if source is R(A,B):
        emit(B, ("R", A))
    else if source is S(B,C):
        emit(B, ("S", C))

REDUCE(join_key B, values):
    separate values tagged R and S
    for each R-value:
        for each S-value:
            emit(combined_tuple(A, B, C))
```

### Q44. Write pseudocode for Group By (A,B), MAX(C).

``` text
MAP(tuple (A,B,C,...)):
    emit((A,B), C)

REDUCE(key=(A,B), C_values):
    max_value = maximum(C_values)
    emit((A,B), max_value)
```

### Q45. Which operations require a Reducer?

**Answer:** In the algorithms shown in the notes, Union needs grouping
to eliminate duplicates; Difference needs grouping to inspect relation
tags; Natural Join needs grouping by the join key; and Grouping &
Aggregation needs grouping by `(A,B)` and computing an aggregate.

### Q46. Why is MapReduce suitable for large relations?

**Answer:** The input can be split among Map workers, while grouping and
final computation are distributed across Reducers. This supports
parallel processing of large datasets.

### Q47. What is the main limitation of Reduce-side Natural Join?

**Answer:** Records with the same join key must be shuffled to the same
Reducer, which can cause substantial network transfer. If a key occurs
very frequently, that Reducer can become a bottleneck.

### Q48. What should you say if asked to explain any relational MapReduce diagram?

**Answer:** Explain it in this order: input tables → Mapper key-value
pairs → Shuffle and Sort grouping → Reducer rule → final output table.
State what the key represents and why the value is needed.

------------------------------------------------------------------------

# PART B --- CAP THEOREM

## B1. Fundamentals

### Q49. What is the CAP theorem?

**Answer:** The CAP theorem states that during a network partition, a
distributed data system cannot guarantee all three
properties---Consistency, Availability, and Partition
Tolerance---simultaneously. The practical choice during a partition is
commonly between CP and AP behavior.

### Q50. What does C stand for?

**Answer:** **Consistency.** In the notes, all clients should see the
same, most recent data regardless of which server they contact.

### Q51. Give an example of Consistency.

**Answer:** If only one item remains and a customer buys it through
Server 1, Server 2 should also show that the item is out of stock rather
than showing stale stock information.

### Q52. What does A stand for?

**Answer:** **Availability.** Every request to a functioning part of the
system receives a response within a reasonable time, even when some
servers are unavailable or cannot communicate.

### Q53. What does P stand for?

**Answer:** **Partition Tolerance.** The system continues operating
despite a network communication break or message loss between parts of
the distributed system.

### Q54. What is a network partition?

**Answer:** A network partition occurs when nodes in a distributed
system cannot communicate with one another because of a network failure
or another communication problem.

### Q55. Why does the CAP choice usually become CP versus AP?

**Answer:** A distributed system must be designed to cope with network
partitions. When a partition occurs, it must choose whether to reject or
delay some requests to preserve consistency (CP) or keep responding
while allowing temporarily inconsistent data (AP).

------------------------------------------------------------------------

## B2. CP, AP, and CA

### Q56. What is a CP system?

**Answer:** A CP system prioritizes **Consistency and Partition
Tolerance**. During a partition, it may reject, delay, or make some
requests unavailable rather than return data that could violate
consistency.

### Q57. What is an AP system?

**Answer:** An AP system prioritizes **Availability and Partition
Tolerance**. It continues responding during a partition, but different
nodes may temporarily return different or outdated data. When
communication is restored, replicas can synchronize.

### Q58. What is a CA system?

**Answer:** A CA system provides Consistency and Availability when there
is no network partition. A single-node database is often used as the
simple example in the notes because it does not have inter-node
partitions to tolerate.

### Q59. Why is CA not a realistic guarantee for a distributed system during a partition?

**Answer:** If distributed nodes cannot communicate, the system cannot
guarantee both that every request receives a response and that every
response reflects the latest globally consistent state. During the
partition, the system must trade off availability or consistency.

### Q60. Compare CP, AP, and CA.

  -------------------------------------------------------------------------------------
  Type              Prioritizes       Trade-off during a      Examples stated in the
                                      partition               notes
  ----------------- ----------------- ----------------------- -------------------------
  CP                Consistency +     Some requests may be    MongoDB, HBase (as listed
                    Partition         unavailable             in the notes)
                    Tolerance                                 

  AP                Availability +    Data may be temporarily Cassandra, CouchDB, Riak,
                    Partition         outdated/inconsistent   DynamoDB (as listed in
                    Tolerance                                 the notes)

  CA                Consistency +     Does not provide        Traditional/single-node
                    Availability      partition tolerance     SQL database example in
                    without a         during a network split  the notes
                    partition                                 
  -------------------------------------------------------------------------------------

*Viva caution:* CAP classifications depend on the exact product,
configuration, and operation. If your examiner expects the notes'
examples, state those examples as given in the notes.

### Q61. What does "eventual consistency" mean in an AP system?

**Answer:** It means replicas may temporarily hold different values
during a partition, but after communication is restored and updates are
exchanged, the replicas are expected to converge if no new conflicting
updates continue.

### Q62. Does CAP mean a database permanently chooses only two properties?

**Answer:** The key limitation concerns what a distributed system can
guarantee **when a network partition occurs**. When there is no
partition, a system may provide both consistency and availability.
Explain the partition scenario rather than simply memorizing "only two."

------------------------------------------------------------------------

## B3. CAP Inventory Scenario from the Notes

### Q63. Explain the e-commerce inventory scenario.

**Answer:** An e-commerce company stores inventory across servers in two
cities. A network failure prevents the servers from communicating while
customers continue placing orders. The company must choose whether to
accept orders using local inventory data or stop/delay orders until
stock information is synchronized.

### Q64. How does the scenario illustrate Consistency?

**Answer:** Both servers should show the same current stock. If Server 1
sells the last product, Server 2 should not continue reporting that the
product is available.

### Q65. How does the scenario illustrate Availability?

**Answer:** Customers should receive responses and be able to place
orders even when the servers cannot communicate.

### Q66. How does the scenario illustrate Partition Tolerance?

**Answer:** The network failure has created a partition. The system
needs a defined behavior even though the servers cannot exchange
updates.

### Q67. What is the CP choice for the inventory system?

**Answer:** Temporarily stop accepting orders, reject some requests, or
wait for synchronization to avoid inconsistent inventory and
overselling. This prioritizes consistency and partition tolerance at the
expense of availability.

### Q68. What is the AP choice for the inventory system?

**Answer:** Continue accepting orders using the inventory data available
locally. This prioritizes availability and partition tolerance, but
servers may temporarily disagree about stock and overselling may occur.

### Q69. Which choice should the company make if overselling must be avoided?

**Answer:** Based on the notes, choose CP behavior: temporarily
sacrifice availability so the system can preserve accurate, consistent
inventory.

### Q70. Which choice should the company make if continuous customer access is the priority?

**Answer:** Choose AP behavior: keep the service available during the
partition, accepting that stock information may temporarily be
inconsistent.

### Q71. What is the conclusion of the inventory scenario?

**Answer:** During a network partition, the company cannot guarantee
both globally consistent inventory and uninterrupted order availability.
It must prioritize the property that best fits its business
requirements.

------------------------------------------------------------------------

# PART C --- NOSQL

## C1. Introduction and Definition

### Q72. What is NoSQL?

**Answer:** NoSQL stands for **"Not Only SQL."** In the notes, it is
described as a database management approach for storing and retrieving
massive amounts of data in distributed environments, with an emphasis on
scalability, performance, availability, and agility.

### Q73. Why was NoSQL developed?

**Answer:** It developed in response to the growing volume of data about
users, objects, and products that must be accessed and processed
frequently. The notes explain that traditional relational systems may be
difficult to scale or change for some modern workloads.

### Q74. Is NoSQL only for unstructured data?

**Answer:** No. According to the notes, NoSQL can be suitable for
structured, semi-structured, and unstructured data. The appropriate
database depends on the data model and workload.

### Q75. What are the four NoSQL data models shown in the notes?

**Answer:** 1. Column-family. 2. Graph. 3. Document. 4. Key-value.

### Q76. Explain a document database.

**Answer:** A document database stores records as documents, often in
formats such as JSON-like documents. Different documents can have
different fields, which supports flexible product or user data.

### Q77. Explain a key-value database.

**Answer:** A key-value database stores values associated with unique
keys. The key is used to retrieve the corresponding value. It is useful
for simple lookups, caching, sessions, and similar access patterns.

### Q78. Explain a column-family database.

**Answer:** A column-family database organizes data into rows and groups
related columns into families. It is designed for distributed storage
and large datasets. HBase is an example listed in the notes.

### Q79. Explain a graph database.

**Answer:** A graph database represents data as nodes and
relationships/edges. It is useful when relationships are central, such
as social networks, recommendations, or connected entities.

### Q80. Name NoSQL examples from the notes.

**Answer:** MongoDB, Cassandra, HBase, and Redis are listed in the
SQL-versus-NoSQL comparison. The CAP section also mentions CouchDB,
Riak, Voldemort, and DynamoDB.

------------------------------------------------------------------------

## C2. SQL versus NoSQL

### Q81. Differentiate SQL and NoSQL as shown in the notes.

  -----------------------------------------------------------------------
  Feature                 SQL / relational        NoSQL
                          database                
  ----------------------- ----------------------- -----------------------
  Meaning                 Structured Query        Not Only SQL
                          Language                

  Data storage            Tables with rows and    Documents, key-value
                          columns                 pairs, graphs, column
                                                  families

  Schema                  Fixed/predefined schema Flexible schema

  Relationships           Supports complex        Often avoids complex
                          relationships using     JOINs through
                          keys and JOINs          data-model design

  Scalability             Notes emphasize         Notes emphasize
                          vertical scaling        horizontal scaling

  Data types              Best suited to          Can handle structured,
                          structured data         semi-structured, and
                                                  unstructured data

  Transactions            Strong ACID             Transaction support
                          transactions are a      varies by database
                          traditional strength    

  Query language          SQL                     Depends on database

  Examples                MySQL, Oracle,          MongoDB, Cassandra,
                          PostgreSQL              HBase, Redis

  Common uses in notes    Banking, payroll,       Big Data, real-time
                          inventory               applications, social
                                                  media, IoT
  -----------------------------------------------------------------------

### Q82. What is vertical scaling?

**Answer:** Vertical scaling means increasing the capacity of one
machine---for example, adding CPU, RAM, or storage.

### Q83. What is horizontal scaling?

**Answer:** Horizontal scaling means adding more machines or nodes to
distribute storage and processing across a cluster.

### Q84. Why do the notes associate NoSQL with horizontal scaling?

**Answer:** Many NoSQL systems are designed to distribute data across
multiple nodes, allowing capacity to grow by adding machines rather than
relying only on a larger single server.

### Q85. Does every NoSQL database lack transactions or consistency?

**Answer:** No. The notes say transaction and consistency support
varies. NoSQL databases have different designs; avoid claiming that
every NoSQL database lacks transactions or guarantees.

### Q86. Why might NoSQL avoid complex JOINs?

**Answer:** Some NoSQL data models store related information together or
organize it for the application's common access pattern. This can reduce
the need for costly cross-table joins, although it may introduce
duplication or other trade-offs.

### Q87. When might SQL be a better choice?

**Answer:** SQL can be suitable when data is strongly structured,
relationships and joins are important, and the application needs
well-defined relational constraints and transaction behavior---for
example, banking or payroll systems as listed in the notes.

### Q88. When might NoSQL be a better choice?

**Answer:** NoSQL may be appropriate when data volume is large, the
schema evolves frequently, data formats vary, or distributed scaling and
availability are important---for example, social media, IoT, and some
real-time applications.

------------------------------------------------------------------------

## C3. CAP Theorem within NoSQL

### Q89. Why is CAP relevant to NoSQL?

**Answer:** Many NoSQL systems distribute data across multiple nodes.
When nodes cannot communicate, the system must decide how to balance
consistent data with continued responses. CAP helps explain these
trade-offs.

### Q90. Define Consistency, Availability, and Partition Tolerance in the NoSQL context.

**Answer:** - **Consistency:** Clients see the same, most recent data
across nodes. - **Availability:** Requests receive a response from
functioning nodes. - **Partition Tolerance:** The system continues
operating despite communication failures between nodes.

### Q91. What does the notes' CAP diagram communicate?

**Answer:** The diagram presents C, A, and P as three desirable
properties and illustrates CP, AP, and CA combinations. Its central
point is that all three cannot be fully guaranteed during a network
partition.

### Q92. What happens to a CP system during a partition?

**Answer:** It may make some nodes or requests unavailable until
consistency can be preserved.

### Q93. What happens to an AP system during a partition?

**Answer:** Nodes continue responding, but some clients may receive
older or different data. The nodes can synchronize when communication
returns.

### Q94. Why is partition tolerance important?

**Answer:** Distributed systems rely on network communication, which can
fail. Partition tolerance defines how the system behaves when some nodes
become unable to communicate.

------------------------------------------------------------------------

## C4. Features of NoSQL

### Q95. List the features of NoSQL from the notes.

**Answer:** - Non-relational data model. - Schema-free or flexible
schema. - Simple API. - Distributed design. - Shared-nothing
environment. - Horizontal scalability. - Ability to use lower-cost
commodity hardware.

### Q96. What does "shared-nothing environment" mean?

**Answer:** Each node performs its work independently and has its own
local resources rather than relying on a single shared disk or memory
resource. Nodes coordinate over the network as required.

### Q97. What is a schema-free data model?

**Answer:** It means records do not all have to follow one rigid,
predefined set of fields. Different documents or records can have
different attributes, which is useful when requirements change.

### Q98. Does schema-free mean no structure at all?

**Answer:** No. Data still has a model or format, but the database may
not require every record to follow one fixed table schema. Applications
may still enforce validation rules.

### Q99. What is a simple API in the context of the notes?

**Answer:** It refers to a relatively direct interface for storing and
retrieving data, such as accessing a value by key or querying a document
collection, depending on the database.

------------------------------------------------------------------------

## C5. Advantages of NoSQL

### Q100. List the advantages of NoSQL databases from the notes.

**Answer:** 1. Flexible Data Model. 2. Evolving Data Model. 3. Elastic
Scalability. 4. High Performance. 5. Open-source.

### Q101. Explain Flexible Data Model.

**Answer:** NoSQL can store different data types and records with
different fields. This is useful when the application combines
structured and unstructured data or when records do not all have
identical attributes.

### Q102. Explain Evolving Data Model.

**Answer:** The schema can evolve as application requirements change,
often without the same table-wide schema migration process used in a
traditional relational database.

### Q103. Explain Elastic Scalability.

**Answer:** NoSQL systems can expand to accommodate growing data and
workloads by adding resources, often horizontally, while aiming to
control cost.

### Q104. Explain High Performance.

**Answer:** The notes describe performance in terms of **throughput**
and **latency**. A suitable NoSQL system can support high request
volumes and low response delays for its target workload.

### Q105. What is throughput?

**Answer:** Throughput is the amount of work or number of requests
processed per unit of time.

### Q106. What is latency?

**Answer:** Latency is the delay between issuing a request and receiving
its response.

### Q107. Explain Open-source as an advantage.

**Answer:** Open-source databases can avoid expensive software licensing
fees and may run on commodity hardware, making deployment more
cost-effective. Operational, support, and infrastructure costs still
need to be considered.

------------------------------------------------------------------------

## C6. Disadvantages of NoSQL

### Q108. List the disadvantages from the notes.

**Answer:** 1. Lack of Standardization. 2. Backup of Database. 3.
Consistency.

### Q109. Explain Lack of Standardization.

**Answer:** NoSQL products differ in their data models, APIs, query
languages, and design. Skills or queries learned for one product may not
transfer directly to another.

### Q110. Explain the backup concern mentioned in the notes.

**Answer:** The notes state that backup can be a drawback and that some
NoSQL backup tools may not provide a sufficiently mature or complete
solution. In practice, backup capabilities differ by product and
version, so the selected database's backup and recovery features must be
evaluated.

### Q111. Explain the consistency concern mentioned in the notes.

**Answer:** Some NoSQL systems trade strict consistency for scalability,
availability, or performance in particular configurations. If
constraints and duplicate prevention are not enforced by the application
or database, repeated data may be accepted. However, consistency and
transaction guarantees vary across NoSQL products.

### Q112. Does every NoSQL database accept duplicate data?

**Answer:** No. The notes use duplicates to illustrate a possible
concern, but actual behavior depends on keys, indexes, constraints, and
the database's data model. Some NoSQL databases enforce uniqueness or
support transactions.

### Q113. How can an organization reduce NoSQL disadvantages?

**Answer:** Select a database that fits the workload, define key and
validation rules, test consistency behavior, implement and test backups
and restores, monitor performance, document the data model, and train
developers on the chosen product.

------------------------------------------------------------------------

## C7. NoSQL Business Drivers

### Q114. What are the four broad business drivers illustrated in the notes?

**Answer:** Volume, Velocity, Variability, and Agility.

### Q115. Explain Volume as a NoSQL business driver.

**Answer:** Volume refers to the large quantity of data that
organizations need to store and query. The notes explain that scaling
out across clusters of commodity machines can support large datasets
beyond the practical capacity of one machine.

### Q116. Why can a faster processor alone fail to solve a Big Data problem?

**Answer:** The dataset may exceed the memory or storage capacity of a
single machine. Horizontal scaling distributes the data and work across
multiple machines rather than only increasing the speed of one
processor.

### Q117. Explain Velocity as a business driver.

**Answer:** Velocity is the speed and frequency at which data arrives
and must be read or written. E-commerce and social-media applications
may receive bursts of requests and need to handle real-time inserts and
updates.

### Q118. Why can bursts of online shopping traffic be challenging?

**Answer:** A large number of simultaneous reads and writes can slow
response times. The notes point out that heavily indexed relational
systems may experience performance costs during high-volume update
periods, and scaling them can be expensive.

### Q119. Explain Variability as a business driver.

**Answer:** Variability means the data's fields or structure may differ
between records or change over time. A fixed relational schema can
become awkward when only a few customers or products have special
attributes.

### Q120. What is a sparse matrix/row issue in the variability example?

**Answer:** If a new column is added for a small group of records, most
rows may have no value for that column. This creates sparse data
representation and may waste space or complicate schema management.

### Q121. Why can changing a relational schema affect availability?

**Answer:** The notes explain that adding a column can require an
`ALTER TABLE` operation and may interfere with active transactions or
require operational coordination. The exact impact depends on the
database engine and migration method.

### Q122. Explain Agility as a business driver.

**Answer:** Agility is the ability to adapt the data model and
application quickly as requirements change. The notes discuss the
complexity of nested queries and object-relational mapping, which can
slow implementation and testing in some relational applications.

### Q123. What is ORM?

**Answer:** ORM stands for Object-Relational Mapping. It maps objects in
application code to relational database tables and translates object
operations into SQL statements. The notes mention Java Hibernate as an
example of an ORM framework.

------------------------------------------------------------------------

## C8. Desirable NoSQL Features that Drive Business

### Q124. List all six desirable features from the notes.

**Answer:** 1. 24 × 7 Data Availability. 2. Location Transparency. 3.
Schema-less Data Model. 4. Modern-day Transaction Analysis. 5.
Architecture that Suits Big Data. 6. Analytics and Business
Intelligence.

### Q125. Explain 24 × 7 Data Availability.

**Answer:** Businesses such as e-commerce sites need to remain
accessible around the clock. A distributed NoSQL architecture with
replicated data can help maintain service when a node fails, depending
on configuration.

### Q126. Explain Location Transparency.

**Answer:** Data can be stored across different servers or geographic
locations while the application accesses it without needing to know the
physical location of each record.

### Q127. Explain Schema-less Data Model as a business feature.

**Answer:** Different records can have different fields. For example,
electronics may have RAM and storage attributes, while clothing may have
size and color. A flexible data model allows these fields to be stored
without forcing every record into the same set of columns.

### Q128. Explain Modern-day Transaction Analysis.

**Answer:** Modern applications generate large volumes of fast-moving
data such as clickstreams, sensor records, and social-media activity.
The notes identify a need to analyze this transactional data at scale
and speed, beyond simple debit/credit transactions.

### Q129. Explain Architecture that Suits Big Data.

**Answer:** NoSQL's distributed architecture can fit data pipelines
involving large volume, high velocity, and varied formats. The diagram
in the notes shows data moving through storage and preprocessing into
Hadoop and key-value storage, with an online query engine and a
site/application consuming the data.

### Q130. Explain Analytics and Business Intelligence.

**Answer:** Customer purchases, reviews, and browsing behavior can be
analyzed to identify buying patterns, recommend products, understand
trends, and support faster business decisions.

------------------------------------------------------------------------

## C9. Scenario-based Viva Questions

### Q131. A large shopping platform receives millions of transactions, reviews, and product updates daily. Product fields vary by category, and customers need continuous access. Give five reasons to adopt NoSQL.

**Answer (matching the CAP Theory PDF):** 1. **24 × 7 Data
Availability:** Distributed and replicated data can support continuous
access. 2. **Location Transparency:** Users can access data without
knowing where it is physically stored. 3. **Schema-less Data Model:**
Different product categories can have different attributes. 4.
**Architecture that Suits Big Data:** Horizontal scaling supports large
volumes of transactions, reviews, and product records. 5. **Analytics
and Business Intelligence:** Purchase and browsing data can be analyzed
for recommendations and business decisions.

### Q132. An inventory database is distributed across two cities. The network fails, but customers are still ordering. Apply CAP.

**Answer:** Partition Tolerance is required because the servers cannot
communicate. The company chooses between: - **CP:** Temporarily stop or
delay some orders to preserve consistent stock information and avoid
overselling. - **AP:** Continue accepting orders using local stock data,
but servers may temporarily disagree and overselling may occur. The
correct choice depends on whether accurate inventory or uninterrupted
order processing is more important.

### Q133. An online store has product records with different attributes. Why may NoSQL be suitable?

**Answer:** A flexible schema can store different attributes for
electronics, clothing, and other product categories. A distributed
architecture can also support growing data volume and access from
multiple regions.

### Q134. A social-media application needs to store posts, user profiles, connections, and activity. Which NoSQL data model could fit?

**Answer:** The choice depends on the access pattern: document databases
can store profile/post documents, graph databases can model user
connections, and key-value stores can support fast lookups or caching.
This is an application of the data-model types shown in the notes.

### Q135. A business wants high availability during network failures. What CAP trade-off should it consider?

**Answer:** It can prioritize AP behavior and continue responding during
partitions, while accepting temporarily stale or inconsistent data. If
strict consistency is more important, CP behavior may make some requests
unavailable during the partition.

### Q136. A database query is slow because data is distributed across many related tables. What should you discuss?

**Answer:** Explain that relational systems support joins, but complex
joins and object-relational mapping can add development or execution
complexity. Some NoSQL models denormalize or store related data together
to optimize common access patterns, though the trade-offs depend on the
workload.

------------------------------------------------------------------------

# PART D --- COMBINED COMPARISONS AND RAPID REVISION

## D1. Important Comparisons

### Q137. Differentiate Union and Difference.

-   **Union:** Combines tuples from either relation and removes
    duplicates.
-   **Difference:** Returns tuples in the first relation but not the
    second.

### Q138. Differentiate Difference and Natural Join.

-   **Difference:** Compares tuple membership across relations.
-   **Natural Join:** Matches records using a common attribute and
    combines their attributes.

### Q139. Differentiate Natural Join and Grouping & Aggregation.

-   **Natural Join:** Combines records from two relations based on
    matching keys.
-   **Grouping & Aggregation:** Groups records by selected attributes
    and calculates a summary such as MAX, SUM, or COUNT.

### Q140. Differentiate CAP Consistency and NoSQL flexible schema.

**Answer:** CAP Consistency concerns whether clients see the same
current data across distributed nodes. Flexible schema concerns whether
records must share one fixed set of fields. They are different concepts.

### Q141. Differentiate availability and scalability.

**Answer:** Availability means the system can respond to requests.
Scalability means the system can handle increased workload by adding
resources or capacity. A system can be scalable without always being
available, and vice versa.

### Q142. Differentiate throughput and latency.

**Answer:** Throughput measures how much work is completed per unit
time. Latency measures how long an individual request takes to receive a
response.

### Q143. Differentiate Volume, Velocity, Variability, and Agility.

-   **Volume:** How much data must be stored or processed.
-   **Velocity:** How quickly data arrives and must be handled.
-   **Variability:** How much data structure or content varies.
-   **Agility:** How quickly the data model/application can adapt to
    changing needs.

### Q144. Why should a business choose a database based on workload rather than the label SQL or NoSQL alone?

**Answer:** Different applications need different combinations of
transaction guarantees, query patterns, schema flexibility, latency,
throughput, availability, and scaling. The database should match those
requirements rather than being selected only because it is SQL or NoSQL.

------------------------------------------------------------------------

## D2. Rapid Revision Table

  -----------------------------------------------------------------------
  Topic                               One-line answer
  ----------------------------------- -----------------------------------
  Relational algebra                  Operations for querying and
                                      manipulating relations

  Union                               Tuples in either relation;
                                      duplicates removed

  Difference R−S                      Tuples in R but not S

  Natural Join                        Combines rows matching common
                                      attributes

  Grouping                            Collects records with the same
                                      group key

  Aggregation                         Calculates MAX, MIN, SUM, COUNT,
                                      AVG, etc.

  Map                                 Emits intermediate key-value pairs

  Shuffle and Sort                    Groups records with the same key

  Reduce                              Applies operation to each key's
                                      group

  CAP theorem                         During a partition, a distributed
                                      system cannot guarantee C, A, and P
                                      together

  Consistency                         Clients see the same current data

  Availability                        Requests receive a response

  Partition Tolerance                 System operates despite
                                      communication partition

  CP                                  Consistency + Partition Tolerance;
                                      may sacrifice availability

  AP                                  Availability + Partition Tolerance;
                                      may allow stale data

  CA                                  Consistency + Availability in the
                                      absence of a partition

  NoSQL                               "Not Only SQL"

  NoSQL models                        Column-family, graph, document,
                                      key-value

  Flexible schema                     Records can have different fields

  Horizontal scaling                  Add nodes to increase capacity

  Vertical scaling                    Increase capacity of one machine

  Throughput                          Work processed per unit time

  Latency                             Delay for a response

  NoSQL advantages                    Flexible/evolving model, elastic
                                      scaling, performance, open-source

  NoSQL disadvantages in notes        Lack of standardization, backup
                                      concerns, consistency trade-offs

  Business drivers                    Volume, Velocity, Variability,
                                      Agility

  Six desirable features              24×7 availability, location
                                      transparency, schema-less model,
                                      modern transaction analysis, Big
                                      Data architecture, analytics/BI
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# Final External Viva Strategy

1.  **For every relational operation:** state the definition, identify
    the key, show Mapper output, explain Shuffle and Sort, state the
    Reducer rule, and write the final result.
2.  **For Union and Difference:** practise the tuples `(1,2)`, `(3,1)`,
    and `(2,1)` from the notes.
3.  **For Natural Join:** clearly identify B as the common key in the
    `R(A,B)` and `S(B,C)` example.
4.  **For Grouping & Aggregation:** remember the exact task in the
    notes: group by `(A,B)` and calculate `MAX(C)`.
5.  **For CAP:** explain the network partition first, then compare CP
    and AP using the inventory example.
6.  **For NoSQL:** memorize the four data models, SQL-vs-NoSQL
    comparison, five advantages/disadvantages as listed, four broad
    business drivers, and six desirable business features.
7.  **For scenario questions:** identify the requirement, select the
    relevant concept, apply it to the scenario, and state the trade-off.
8.  **If you forget an answer:** explain the definition, working,
    example, and limitation. This is better than giving only a memorized
    phrase.

**Source note:** Main content is based on the three supplied PDFs. The
relational-operations PDF consists of handwritten diagrams; examples
above use the legible tuples and algorithms shown in those pages. The
CAP/NoSQL examples preserve the categories and terminology used in the
notes. Product capabilities can vary, so database examples are presented
as the notes' examples rather than universal classifications.
