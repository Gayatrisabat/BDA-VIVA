# 7. Algorithms and Numericals

## Numerical 1: Number of HDFS blocks

**Question:** A 500 MB file uses a block size of 128 MB. How many blocks are required?

Formula:

`Number of blocks = ceil(File size / Block size)`

Calculation:

`ceil(500 / 128) = ceil(3.90625) = 4 blocks`

Distribution: 128 MB + 128 MB + 128 MB + 116 MB = 500 MB.

The final block can be smaller than the configured block size.

## Numerical 2: Replication storage

**Question:** A 500 MB file uses 128 MB blocks and replication factor 3.

- Logical blocks: `ceil(500 / 128) = 4`
- Block copies: `4 × 3 = 12`
- Approximate physical storage: `500 × 3 = 1500 MB`

This excludes metadata, checksums, and other overhead and assumes all replicas are placed successfully.

## Numerical 3: Multiple files

Files: 100 MB, 260 MB, and 300 MB. Block size: 128 MB.

- File 1: `ceil(100 / 128) = 1`
- File 2: `ceil(260 / 128) = 3`
- File 3: `ceil(300 / 128) = 3`

Total = `1 + 3 + 3 = 7 blocks`.

Calculate each file separately because each file has its own final block.

## MapReduce word-count algorithm

### Input
```text
big data big
data hadoop
```

### Map phase
The mapper emits `(word, 1)` for every word.

```text
(big, 1)
(data, 1)
(big, 1)
(data, 1)
(hadoop, 1)
```

### Shuffle and sort
```text
big     -> [1, 1]
data    -> [1, 1]
hadoop  -> [1]
```

### Reduce phase
```text
big     -> 2
data    -> 2
hadoop  -> 1
```

### Pseudocode
```text
MAP(record):
    for each word in record:
        emit(word, 1)

REDUCE(word, values):
    total = 0
    for each value in values:
        total = total + value
    emit(word, total)
```

## Common algorithm questions
**Why is MapReduce parallel?** Different input splits can be processed by multiple mappers; different keys can be handled by reducers in parallel.

**Why shuffle and sort?** It groups intermediate values by key before reduction.

**Is MapReduce the same as Hadoop?** No. MapReduce is a processing model/framework; Hadoop is a broader ecosystem.
