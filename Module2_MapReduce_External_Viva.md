# Module 2: MapReduce

## External Viva Questions and Answers --- Basic to Advanced

**Source:** Supplied *Module 2: Mapreduce* PDF (57 pages). This guide
follows the module's topics and terminology, including DFS, MapReduce
pipeline, fault handling, word count, relational algebra, and matrix
multiplication.

## Contents

1.  Distributed File System (DFS)
2.  DFS services and cluster organization
3.  MapReduce fundamentals and execution pipeline
4.  Combiner, Partitioner, and Distributed Cache
5.  Failure handling
6.  Word-count algorithm and numerical
7.  Relational algebra operations
8.  Matrix multiplication
9.  Tricky questions and rapid revision

------------------------------------------------------------------------

# 1. Distributed File System (DFS)

### Q1. What is a Distributed File System?

A Distributed File System (DFS) stores files across multiple computers
or servers rather than one computer. Users can access and manage the
files as if they were stored in one location, even though the files are
physically distributed.

### Q2. List the features of DFS.

-   Stores data on multiple file servers.
-   Allows access from network-connected computers.
-   Supports sharing among multiple users.
-   Provides centralized management of distributed files.
-   Offers authentication and access control.
-   Improves reliability and fault tolerance.

### Q3. Why do we need DFS?

DFS supports large-scale storage, collaboration, centralized access, and
fault tolerance. Files can be divided into blocks and distributed across
multiple servers, increasing available storage capacity.

### Q4. Differentiate a local file system and DFS.

A local file system stores files on one computer's storage. A DFS stores
files across networked computers while presenting a unified way to
access them.

### Q5. What is fault tolerance?

Fault tolerance is the ability of a system to continue operating or
recover when a component fails. DFS can keep multiple replicas of data
on different servers so a surviving copy can be accessed.

### Q6. What are the three DFS services in the notes?

1.  Storage Service.
2.  True File Service.
3.  Name Service.

### Q7. Explain Storage Service.

It stores files across multiple servers, divides large files into
smaller blocks, distributes blocks, and maintains copies for fault
tolerance. The notes give a 500 MB file as an example that may be
divided into smaller blocks and stored on different servers.

### Q8. Explain True File Service.

It gives users a single, unified view of files across servers. It
supports opening, reading, writing, and deleting files while hiding
their physical locations. For example, a user opens `Report.pdf` without
knowing which server stores it.

### Q9. Explain Name Service.

It maintains file names and maps names to their storage locations so
users can locate files. The notes use `EmployeeData.txt` as an example.

### Q10. Differentiate the three DFS services.

  Service             Main function
  ------------------- --------------------------------------------------
  Storage Service     Stores blocks and replicas
  True File Service   Provides unified file access and file operations
  Name Service        Maps file names to storage locations

### Q11. State four uses of DFS.

Centralized file access, data sharing, storage of large data, and fault
tolerance.

### Q12. What is a compute node?

A compute node is an individual computer or server in a cluster that
performs computation and processing tasks.

### Q13. What is a rack?

A rack is a physical enclosure containing multiple compute nodes. The
notes mention a typical range of about 8--64 nodes per rack.

### Q14. Differentiate intra-rack and inter-rack connections.

-   **Intra-rack:** Communication between nodes in the same rack, often
    using rack-level Ethernet.
-   **Inter-rack:** Communication between different racks through a
    high-speed network or switch.

### Q15. Why should replicas be placed on different racks?

This reduces the chance that a single rack failure will make every copy
of a data block unavailable.

### Q16. What is a chunk?

A chunk is a piece of a large file. The notes describe chunks as
typically 64 MB in their example, with multiple copies stored on
different compute nodes. Chunk size and replication degree depend on
configuration.

### Q17. Why are very large, rarely updated files a good DFS workload?

They can be split across nodes and read for large-scale computation. The
notes caution that a frequently updated airline reservation system is
not a suitable example of the typical large-file, read-oriented DFS
workload.

### Q18. What is the role of a master node/NameNode?

It maintains file-system metadata and tracks where file chunks or blocks
are stored. It normally manages locations and namespace information
rather than storing the actual file contents.

------------------------------------------------------------------------

# 2. MapReduce Fundamentals

### Q19. What is MapReduce?

MapReduce is a programming model/framework for distributed and parallel
processing of large datasets. Map tasks transform input records into
intermediate key-value pairs; the framework groups values by key; Reduce
tasks aggregate or process those values to create output.

### Q20. What principle does MapReduce follow?

**Divide and conquer.** Large input is divided into smaller chunks,
processed in parallel, and the partial results are combined.

### Q21. Explain the MapReduce flow.

1.  Input data is divided into logical input splits.
2.  Map tasks read records and emit intermediate key-value pairs.
3.  Shuffle and Sort transfers and groups values with the same key.
4.  Reduce tasks process grouped values.
5.  Final output is written to storage such as HDFS.

### Q22. What is a key-value pair?

It contains a key that identifies or groups a record and a value
associated with that key. In word count, the Mapper emits `(Refund, 1)`
for each occurrence of "Refund".

### Q23. What does a Mapper do?

It reads assigned input records, applies user-defined logic, and emits
zero or more intermediate key-value pairs. Different Mappers can work
independently on different input splits.

### Q24. What does a Reducer do?

It receives values grouped by key, applies user-defined aggregation or
computation, and emits zero or more final key-value pairs.

### Q25. Is a Reducer compulsory?

No. The Mapper is mandatory, while a Reduce phase is optional for jobs
whose result can be produced in the Map phase.

### Q26. Explain this expression:

`(k1, v1) → Map → (k2, v2) → Shuffle/Sort → (k2, [v2, v2, ...]) → Reduce → (k3, v3)`

The Mapper transforms an input pair into intermediate pairs. Shuffle and
Sort groups all values with the same key `k2`. The Reducer processes the
grouped values and emits the final result.

### Q27. What is data locality?

Data locality means running a Map task on or near the node that stores
the required input data. It can reduce network transfer and improve
efficiency.

### Q28. Explain the Map process in the `Feedback.txt` example.

The client submits a job to count "Refund". The framework locates file
blocks and assigns Map tasks to workers. Each Mapper processes its local
block and emits `(Refund, 1)` whenever it finds the word. Task progress
is monitored, and intermediate output is stored temporarily before being
shuffled to Reducers.

### Q29. Explain the Reduce process in the same example.

The Reducer gathers all values associated with `Refund`, adds them to
obtain the total count, and writes the result to `Results.txt` in HDFS.
The client can then read the result.

------------------------------------------------------------------------

# 3. MapReduce Execution Pipeline

### Q30. Name the pipeline components in the notes.

Driver, Input Data/InputFormat, Mapper, Shuffle and Sort, Reducer, and
Distributed Cache.

### Q31. What is the Driver?

The Driver is the main program that initializes and manages the job. It
configures Mapper, Reducer, Combiner, and Partitioner classes; specifies
input/output formats; submits the job; and monitors its status.

### Q32. What is InputFormat?

InputFormat handles how input data is read and divided into InputSplits.
It helps determine the number of Map tasks and identify suitable
locations for execution.

### Q33. What is an InputSplit?

An InputSplit is a logical portion of input assigned to a Mapper. It is
not necessarily identical to an HDFS block: a block is a physical
storage unit, while a split is a logical processing unit.

### Q34. What are the functions of a Mapper?

Read assigned records, process them using the map function, and generate
intermediate key-value pairs.

### Q35. Explain Shuffle and Sort.

It transfers intermediate data from Mappers to the appropriate Reducers,
groups values with the same key, and sorts keys before sending them to
Reducers.

### Q36. Explain the `Refund` shuffle example.

**Before shuffle:**

``` text
(Refund, 1)
(Refund, 1)
(approved, 1)
(delayed, 1)
```

**After grouping:**

``` text
Refund   -> [1, 1]
approved -> [1]
delayed  -> [1]
```

The Reducer can count each word by summing its grouped values.

### Q37. What is a Partitioner?

A Partitioner decides which Reducer receives each intermediate key-value
pair. All values for the same key must go to the same Reducer.

### Q38. What is a Combiner?

A Combiner performs optional local aggregation of Mapper output before
network transfer. For word count, it may combine `(Refund,1)` and
`(Refund,1)` into `(Refund,2)`, reducing the data shuffled.

### Q39. Is the Combiner guaranteed to run?

No. It is an optional optimization. The algorithm must remain correct
even if the framework does not run the Combiner.

### Q40. Differentiate Combiner and Reducer.

  -----------------------------------------------------------------------
  Combiner                            Reducer
  ----------------------------------- -----------------------------------
  Optional local optimization         Performs final per-key computation

  Usually aggregates mapper output    Receives grouped data after shuffle
  locally                             

  Reduces intermediate transfer       Produces final job output

  May not run                         Required when a Reduce phase is
                                      defined
  -----------------------------------------------------------------------

### Q41. What is Distributed Cache?

It makes common resources---such as configuration files, lookup tables,
or shared libraries---available to Mapper and Reducer tasks across the
cluster. It avoids manually copying the same resource separately to
every node.

### Q42. What does the MapReduce framework manage?

It coordinates execution, selects nodes for Map tasks, starts and
monitors Mappers, shuffles and sorts output, selects Reducer locations,
transfers data to Reducers, monitors execution, and reschedules failed
tasks.

------------------------------------------------------------------------

# 4. Fault Tolerance and Node Failures

### Q43. Why are node failures expected in Hadoop?

A cluster contains many machines and network components, so hardware or
process failures can occur. The framework is designed to detect failures
and recover where possible.

### Q44. List the failure-handling techniques in the notes.

1.  Task re-execution.
2.  Speculative execution.
3.  Heartbeat mechanism.
4.  HDFS data replication.

### Q45. What is task re-execution?

When a Map or Reduce task fails, the framework reassigns the task to
another available node and runs it again.

### Q46. What is speculative execution?

If a task is much slower than other tasks, the framework may start a
duplicate copy on another node. The copy that finishes first is accepted
and the other is terminated.

### Q47. What is a straggler?

A straggler is a task that runs much more slowly than similar tasks,
possibly because of hardware or network problems.

### Q48. What is a heartbeat?

A worker periodically sends a heartbeat to the master to indicate it is
active. If heartbeats are missing for a configured period, the master
may consider the worker unavailable and reschedule its tasks.

### Q49. Explain HDFS replication in fault recovery.

HDFS stores multiple copies of data blocks on different DataNodes. If
one DataNode fails, a surviving replica can provide the data. New
replicas may be created to restore the desired replication factor.

### Q50. What happens during a Mapper node failure?

An incomplete Map task is restarted on another node that can access the
required input data.

### Q51. What happens during a Reducer node failure?

The failed Reduce task can be assigned to another available worker and
restarted.

### Q52. What happens during a DataNode failure?

The NameNode detects missing heartbeats. Data can be read from another
DataNode containing a replica, and new replicas may be created if
needed.

### Q53. What happens during a TaskTracker failure?

In the older architecture described in the notes, the JobTracker marks
tasks running on the failed TaskTracker as failed and reschedules them
on another TaskTracker.

### Q54. Differentiate task re-execution and speculative execution.

Task re-execution responds to a failed task. Speculative execution
responds to a task that is still running but unusually slowly.

### Q55. Differentiate data replication and task re-execution.

Replication keeps copies of stored data blocks for data availability.
Task re-execution runs failed computation again so the job can progress.

### Q56. What are JobTracker and TaskTracker?

They are components of the older Hadoop MapReduce Version 1 (MRv1)
architecture. The JobTracker coordinated jobs and assigned tasks;
TaskTrackers executed Map and Reduce tasks on worker nodes. Newer Hadoop
uses YARN components such as ResourceManager and NodeManager.

------------------------------------------------------------------------

# 5. Word-Count Algorithm and Numericals

### Q57. State the word-count problem.

Given a large text corpus, count how many times each word occurs.

### Q58. Write pseudocode for word count.

``` text
MAPPER(document):
    for each word in document:
        emit(word, 1)

REDUCER(word, values):
    total = 0
    for each value in values:
        total = total + value
    emit(word, total)
```

### Q59. Solve this numerical.

**Input:**

``` text
Refund approved
Refund delayed
```

**Map output:**

``` text
(Refund, 1)
(approved, 1)
(Refund, 1)
(delayed, 1)
```

**Shuffle/Sort:**

``` text
Refund   -> [1, 1]
approved -> [1]
delayed  -> [1]
```

**Reduce output:**

``` text
Refund   -> 2
approved -> 1
delayed  -> 1
```

### Q60. Solve the fruit word-count example shown in the notes.

Input lines:

``` text
Apple Orange Mango
Orange Grapes Plum
Apple Plum Mango
Apple Apple Plum
```

Final counts:

``` text
Apple  -> 4
Orange -> 2
Mango  -> 2
Grapes -> 1
Plum   -> 3
```

Each occurrence emits `(word,1)`; shuffle groups values by word; Reduce
sums them.

### Q61. Why does the Mapper emit value 1?

Each emitted `1` represents one occurrence. Summing all values for a
word produces its total count.

### Q62. What is the role of a Combiner in word count?

It can add local counts before shuffle. For example, two local pairs
`(Refund,1)` may be combined to `(Refund,2)`. The Reducer then combines
partial counts from different Mappers.

### Q63. What is the time complexity of word count?

A simple scan is generally O(N) in input size for processing characters
or tokens. Actual distributed elapsed time also depends on split
creation, shuffle, sorting, network/disk I/O, scheduling, and data skew.

------------------------------------------------------------------------

# 6. Relational Algebra Using MapReduce

### Q64. What is relational algebra?

Relational algebra is a set of operations used to query and manipulate
relations (tables). The notes cover selection, projection, union,
intersection, difference, natural join, and grouping/aggregation.

### Q65. What is selection?

Selection filters rows/tuples that satisfy a condition. For example, for
condition `B = 2`, only tuples with B equal to 2 are selected.

### Q66. How is selection implemented?

The Mapper tests each tuple `t`. If it satisfies condition `C`, it emits
`(t,t)`. The Reduce operation may simply pass the pair through; the main
filtering work occurs in Map.

``` text
map(key, value):
    for each tuple t:
        if t satisfies condition C:
            emit(t, t)

reduce(key, values):
    emit(key, key)
```

### Q67. What is projection?

Projection selects specified columns/attributes from each tuple. For
example, projecting relation `R(A,B,C)` onto `B,C` removes attribute A.

### Q68. Why can projection require Reduce?

Different original tuples can become identical after columns are
removed. Shuffle groups identical projected tuples and Reduce emits one
copy, eliminating duplicates.

### Q69. How is projection implemented?

The Mapper creates a projected tuple `ts` and emits `(ts,ts)`. The
Reducer emits one pair for each key `ts`, eliminating duplicate
projected tuples.

### Q70. What is union?

Union combines tuples from relations R and S. The notes require
compatible schemas. Mapper emits `(t,t)` for every tuple; Reducer emits
one copy per tuple key to eliminate duplicates.

### Q71. What is intersection?

Intersection returns only tuples present in both R and S. Both relations
emit tuple keys. The Reducer emits a tuple only if it has evidence that
the tuple occurs in both relations.

### Q72. What is difference R − S?

It returns tuples present in R but not in S. The Mapper marks each
tuple's source by emitting `(t,R)` for tuples from R and `(t,S)` for
tuples from S. The Reducer emits t only if it appears in R and not S.

### Q73. Compare union, intersection, and difference.

  Operation              Result
  ---------------------- ---------------------------
  Union `R ∪ S`          Tuples in either relation
  Intersection `R ∩ S`   Tuples in both relations
  Difference `R − S`     Tuples in R but not in S

### Q74. What is natural join?

Natural join combines tuples from two relations when their common
attributes have matching values. The resulting tuple contains the
relevant attributes from both relations.

### Q75. How is natural join implemented using MapReduce?

Mapper emits records keyed by the join attribute and marks which
relation each record came from. Shuffle groups records by join key.
Reducer separates the two relations' records and combines compatible
tuples that share the key.

### Q76. Give a natural join example.

If `R(A,B)` contains `(1,2)` and `S(B,C)` contains `(2,3)`, the shared
attribute B matches at value 2. The joined result is `(A=1, B=2, C=3)`.

### Q77. What is grouping and aggregation?

Grouping collects records with the same key or grouping attributes.
Aggregation calculates a summary per group, such as count, sum, average,
minimum, or maximum.

### Q78. How is grouping and aggregation implemented?

Mapper emits the grouping attribute as key and the value to aggregate as
value. Shuffle groups records by key, and Reduce calculates the
aggregate for each group.

### Q79. Differentiate selection and projection.

Selection filters **rows** by a condition. Projection selects
**columns/attributes**.

### Q80. Differentiate Map-side join and Reduce-side join.

A Reduce-side join shuffles records with the same join key to Reducers
for matching. A Map-side join joins in the Map phase when the input data
layout/partitioning permits it, potentially avoiding shuffle.

### Q81. What is an in-memory join?

It uses a suitable dataset or lookup structure held in memory to match
records. It can avoid some distributed shuffle, but feasibility depends
on dataset size and available memory.

### Q82. Why might complex relational queries need multiple MapReduce jobs?

A query may require filtering, joining, grouping, sorting, and
aggregation in multiple stages. The output of one job can become the
input of another. The notes give "Top 10 URLs by average time spent" as
an example.

------------------------------------------------------------------------

# 7. Matrix Multiplication Using MapReduce

### Q83. Can matrix multiplication be performed with MapReduce?

Yes. Map tasks emit values needed for each output cell, and Reduce tasks
group and sum all contributions to that cell.

### Q84. What are the dimensions of matrix multiplication?

If A has dimensions L × M and B has dimensions M × N, then C = AB has
dimensions L × N. The inner dimensions must match.

### Q85. State the matrix multiplication formula.

\[ C\[i,k\] = `\sum`{=tex}\_{j=1}\^{M} A\[i,j\] `\times `{=tex}B\[j,k\]
\] Each output cell is the sum of products of the corresponding row of A
and column of B.

### Q86. What key is used for an output cell?

The coordinates `(i,k)` are used as the key, so all values contributing
to `C[i,k]` are grouped for the same Reducer key.

### Q87. Explain the Map phase.

For each element `A[i,j]`, emit it for each output column k using key
`(i,k)`. For each element `B[j,k]`, emit it for each output row i using
the same key `(i,k)`. This groups values needed to compute each output
cell.

### Q88. Explain the Reduce phase.

For each key `(i,k)`, the Reducer matches A and B values by the shared
index j, computes `A[i,j] × B[j,k]`, sums those products, and emits the
final cell value.

### Q89. Write simplified Reduce pseudocode.

``` text
reduce(key=(i,k), values):
    collect A[i,j] values by j
    collect B[j,k] values by j
    result = 0
    for each matching index j:
        result = result + A[i,j] * B[j,k]
    emit((i,k), result)
```

### Q90. Solve a matrix multiplication numerical.

Let \[ A =
```{=tex}
\begin{bmatrix}1 & 2\\3 & 4\end{bmatrix}
```
, `\quad`{=tex} B =
```{=tex}
\begin{bmatrix}5 & 6\\7 & 8\end{bmatrix}
```
\]

\[ C\[1,1\] = 1(5)+2(7)=19 \] \[ C\[1,2\] = 1(6)+2(8)=22 \] \[ C\[2,1\]
= 3(5)+4(7)=43 \] \[ C\[2,2\] = 3(6)+4(8)=50 \]

Therefore, \[ C=AB=
```{=tex}
\begin{bmatrix}19 & 22\\43 & 50\end{bmatrix}
```
\]

### Q91. Why is one key associated with each output cell?

Every product needed for a particular output cell has the same
coordinates `(i,k)`. Grouping by this key lets one Reduce calculation
sum the contributions for that cell.

### Q92. Compare word count and matrix multiplication in MapReduce.

Word count uses the word as key and sums occurrence counts. Matrix
multiplication uses output-cell coordinates `(i,k)` as key and sums
products of matching matrix elements.

------------------------------------------------------------------------

# 8. Tricky External-Viva Questions

### Q93. Does every Mapper output go to every Reducer?

No. The Partitioner routes each key to a particular Reducer. All values
for the same key must reach the same Reducer.

### Q94. Can a Mapper emit zero key-value pairs?

Yes. The notes state that a Mapper may emit zero or more pairs. For
selection, it emits nothing for tuples that do not satisfy the
condition.

### Q95. Can a Reducer emit multiple pairs?

Yes. A Reducer may emit zero or more output pairs depending on the
algorithm.

### Q96. Why is shuffle expensive?

It can involve network transfer, serialization, sorting, grouping, and
disk I/O. A valid Combiner can reduce the volume of intermediate data.

### Q97. What if all records have the same key?

They are sent to the same Reducer, which can become a bottleneck. This
is an example of data skew.

### Q98. Is speculative execution always beneficial?

No. It may reduce delay from stragglers but consumes extra resources
because duplicate tasks run simultaneously.

### Q99. Why is the Combiner not the same as the Reducer?

The Combiner is an optional local optimization. The Reducer performs the
final per-key computation. The algorithm must be correct even if the
Combiner is not run.

### Q100. Can selection be done without a meaningful Reduce phase?

Yes. Selection can be completed mostly in Map by emitting only matching
tuples.

### Q101. Why is Reduce useful for projection?

Projection may produce duplicate tuples after removing attributes.
Reduce eliminates duplicates by emitting one copy per projected key.

### Q102. What condition is required for union?

The relations must have compatible schemas.

### Q103. What does R or S mean in difference operation values?

It identifies the relation a tuple came from, allowing the Reducer to
emit tuples that appear in R but not S.

### Q104. What are the limitations of MapReduce?

Shuffle and disk I/O can be expensive, job startup adds overhead,
iterative algorithms may require repeated jobs, and complex workflows
can require multiple stages. It is suited to batch processing but may
not be ideal for low-latency interactive work.

------------------------------------------------------------------------

# Rapid Revision Sheet

  Topic                    One-line answer
  ------------------------ --------------------------------------------------------------
  DFS                      Stores files across computers while providing unified access
  Storage Service          Stores blocks and replicas
  True File Service        Provides unified file access and operations
  Name Service             Maps names to locations
  Compute node             A server that performs processing
  Intra-rack               Communication within one rack
  Inter-rack               Communication across racks
  Chunk                    A part of a large file
  MapReduce                Map → Shuffle/Sort → Reduce
  Mapper                   Emits intermediate key-value pairs
  Reducer                  Aggregates grouped values by key
  Driver                   Configures and submits the job
  InputSplit               Logical input unit assigned to a Mapper
  Partitioner              Chooses the Reducer for a key
  Combiner                 Optional local aggregation
  Distributed Cache        Shares resources with tasks across nodes
  Task re-execution        Restarts failed tasks
  Speculative execution    Runs a duplicate of a slow task
  Heartbeat                Periodic signal that a node is alive
  HDFS replication         Stores multiple copies of blocks
  Selection                Filters rows
  Projection               Selects columns and removes duplicates if needed
  Union                    Tuples in either relation
  Intersection             Tuples in both relations
  Difference               Tuples in R but not S
  Natural join             Combines rows matching common attributes
  Matrix multiplication    (C\[i,k\]=`\sum`{=tex}\_j A\[i,j\]B\[j,k\])
  JobTracker/TaskTracker   Older Hadoop MRv1 terminology

## Final viva strategy

1.  Draw the pipeline: Input → InputFormat/InputSplits → Mapper →
    Shuffle/Sort → Reducer → Output.
2.  Practice the Refund word-count numerical and fruit-count example.
3.  Memorize Combiner, Partitioner, and Distributed Cache.
4.  Explain all four failure-handling techniques.
5.  Practice selection, projection, union, intersection, difference,
    natural join, and grouping/aggregation.
6.  For matrix multiplication, know dimensions, formula, key `(i,k)`,
    and Reduce summation.
7.  Answer in this order: definition → steps/working → example →
    advantage or limitation.

**Source note:** This guide follows the supplied 57-page *Module 2:
Mapreduce* PDF, including its diagrams of the MapReduce pipeline, word
count, Combiner/Shuffle, failure handling, relational operations, and
matrix multiplication. The older JobTracker/TaskTracker terminology has
been preserved where it appears in the notes.
