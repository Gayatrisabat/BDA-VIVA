# 9. Case Studies and Limitations

## Walmart
Walmart uses analytics and data mining to identify purchasing patterns, support recommendations, improve the shopping experience, and increase logistics efficiency.

**Key concepts:** Data mining, recommendations, inventory/logistics optimisation.

## Uber
Uber analyses geographic demand, driver availability, and service usage. Dynamic pricing can respond to demand and supply conditions.

**Key concepts:** Demand forecasting and dynamic pricing.

## Netflix
Netflix analyses viewing history, ratings, and playback behaviour to recommend shows and movies and support content decisions.

**Key concept:** Recommendation systems.

## eBay
eBay must analyse rapidly arriving data and make information accessible while maintaining security and permissions.

**Key concepts:** Streaming analytics and data governance.

## RetailMax
A retailer wants recommendations, customer-behaviour analysis, and delivery-delay detection.

Potential limitations:
- MapReduce may be too slow for real-time recommendations.
- Many small files can cause HDFS metadata overhead.
- Iterative analytics may be inefficient.
- Cluster tuning and resource management can become complex.

Possible improvements include streaming ingestion, low-latency serving systems, and processing engines suited to the workload.

## FinTrack Solutions
A FinTech company needs real-time fraud detection, continuous behaviour monitoring, low-latency alerts, and support for transaction streams.

### Hadoop limitations in this case
1. MapReduce is batch-oriented and can have high latency.
2. Millions of small files create metadata overhead.
3. Iterative machine-learning computations can be inefficient.
4. Streaming requires additional tools.
5. Growing clusters need monitoring, tuning, and resource allocation.

### Proposed architecture from the course notes
- **Kafka:** Transaction-event ingestion.
- **Spark:** Low-latency processing and machine-learning workloads.
- **AWS:** Scalable cloud storage and processing.

## Healthcare case
Incomplete and duplicate patient records are a data-quality problem, related to veracity.

Possible steps:
- Validate records.
- Standardise formats.
- Identify and handle missing values.
- Remove duplicates carefully.
- Monitor data quality and protect sensitive information.

## Smart traffic case
Traffic sensors and GPS devices generate continuously changing data. **Velocity** is important when data must be analysed as it arrives.

## General Big Data challenges
- Privacy and security
- Data quality
- Data integration
- Storage cost and scale
- Complexity of analysis
- Shortage of skilled professionals

## Batch vs. stream processing

| Batch | Stream |
|---|---|
| Processes accumulated data in groups | Processes continuously arriving events |
| Often suited to historical reports | Suited to live monitoring and alerts |
| Example: daily sales analysis | Example: live fraud detection |

## Hadoop MapReduce vs. Spark

| Hadoop MapReduce | Apache Spark |
|---|---|
| Traditional batch-oriented processing | General-purpose engine for batch and streaming-related workloads |
| Often writes intermediate results to disk | Can cache data in memory |
| Iterative processing can be expensive | Often efficient for iterative analytics |
| Commonly uses HDFS storage | Can use HDFS and other storage systems |

Spark can work with Hadoop storage and resource-management components; it does not mean Hadoop and Spark are mutually exclusive.
