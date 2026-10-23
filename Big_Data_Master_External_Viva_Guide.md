# Big Data Analytics — Master External Viva Guide
## Modules 4, 5 and 6 + CURE Algorithm + Module 4 & 5 Numericals

**Purpose:** Concise, non-repetitive external viva preparation from all five uploaded PDFs. Questions are grouped by topic, and repeated definitions are combined. Source terminology and worked examples are preserved. Where a PDF does not provide a complete formula, numerical, or algorithm detail, that limitation is stated rather than silently invented.

## Contents
1. Module 4 — Mining Data Streams
2. Module 5 — Finding Similar Items and Clustering
3. CURE Algorithm — Customer Clustering Case Study
4. Module 4 & 5 Numericals — Bloom Filter, LCS/Edit Distance, Decaying Window
5. Module 6 — Real-Time Big Data Models
6. Collaborative Filtering — Complete Worked Numerical
7. Networks as Graphs and Girvan–Newman Algorithm
8. High-value comparison questions
9. Rapid formula sheet and last-minute revision

---

# 1. Module 4 — Mining Data Streams

## 1.1 Stream Data Model and DSMS

### Q1. What is a Stream Data Model?
A Stream Data Model represents and processes data generated continuously and rapidly over time. Unlike traditional databases, which generally store data before querying it, a stream model processes data as it arrives.

**Example from the notes:** A temperature sensor emits 25°C, 26°C, 26°C, 27°C and 28°C at successive timestamps. These readings form a continuous data stream.

### Q2. What is a Data-Stream-Management System (DSMS), and how does it differ from a DBMS?
A **DSMS** receives, processes, analyzes and responds to continuously arriving data streams.

| DBMS | DSMS |
|---|---|
| Mainly works on stored data | Works on continuously arriving data |
| Data is relatively static | Data changes continuously |
| Query usually runs when requested | Queries can run continuously |
| Example: student database | Example: stock-price monitoring |
| Usually stores complete data | May process data in real time |

### Q3. What are the seven layers of DSMS architecture?
1. **Data Source Layer:** Collects and parses data from sensors, social media, financial markets and stock markets.
2. **Data Ingestion Layer:** Controls data flow, buffering and routing; bridges source and processing layers.
3. **Processing Layer:** The functional heart of the system. Filters, transforms, aggregates and enriches streams; detects patterns and derives insights. The notes mention Apache Flink and Apache Storm.
4. **Storage Layer:** Stores processed data in NoSQL or distributed databases and supports durability and availability during failures.
5. **Querying Layer:** Supports ad hoc and standard queries through SQL-like languages or programming APIs.
6. **Visualization and Reporting Layer:** Presents results as charts, pie charts and histograms and supports reports.
7. **Integration Layer:** Connects DSMS applications to traditional systems, business-intelligence tools, data warehouses, ML applications and NLP applications.

**Architecture diagram, Module 4 page 7:** Input streams enter the stream processor. Standing queries run within the processor; ad hoc queries can be submitted when needed. Limited working storage and archival storage are shown separately, and processed results leave as an output stream.

### Q4. What are standing and ad hoc queries?
A **standing query** remains registered and active, processing new stream items continuously. An **ad hoc query** is submitted when a user needs a particular answer.

### Q5. Differentiate one-time and continuous stream queries.
- **One-time query:** Runs once on data available at that moment and then finishes. Example: average temperature of readings received so far.
- **Continuous query:** Remains active and processes new data as it arrives. Example: continuously display temperatures above 30°C.

For the stream `25, 27, 31, 29, 35, 32`, the query “display temperatures greater than 30°C” outputs **31, 35, 32**.

## 1.2 Window-based queries

### Q6. Why are windows used in stream processing?
A stream may be infinite, so repeatedly processing its complete history is impractical. A window restricts a query to a defined portion of recent data.

- **Time-based window:** Contains data from a specified period, e.g. the last five minutes.
- **Count-based window:** Contains the last N records, e.g. the last five stock prices.
- **Sliding window:** Moves forward as new data arrives; old records leave and new records enter.

**Count-window example:** Current window `100, 102, 105, 103, 107`. When 110 arrives, the new last-five window is `102, 105, 103, 107, 110`; 100 is removed.

**Average numerical:** For `10, 20, 30, 40, 50`, average = \((10+20+30+40+50)/5=30\). After 60 arrives, window `20, 30, 40, 50, 60`, average = \(200/5=40\).

## 1.3 Stream sources and processing issues

### Q7. Give examples of stream sources and explain their usefulness.
- **Image data:** Satellites can transmit very large image streams; surveillance cameras produce images repeatedly.
- **Internet/web traffic:** Search queries and website clicks form streams. Increased searches for “sore throat” may indicate a health trend; a sudden click increase may indicate a news event or a broken link.

### Q8. What is the unbounded-memory problem? How can it be addressed?
A stream can continue indefinitely. Storing every item forever would require continuously growing memory/storage. The notes suggest:
- Discard old data.
- Store only relevant data.
- Maintain summaries.
- Process data using windows.

### Q9. What is approximate query answering?
It produces an estimate when processing every item in a high-volume, fast stream is too expensive or slow. Techniques listed in the notes are **sampling, data summaries and randomized algorithms**.

**Example:** From 10 lakh transactions, analyze a representative sample of 10,000 to estimate the percentage of fraudulent transactions. The answer may be less exact but faster.

### Q10. What is batch processing, and how is it different from sampling?
**Batch processing** collects records into small groups called batches and processes each group together. **Sampling** selects a smaller representative portion of the stream for processing. Both can improve efficiency; sampling may produce approximate results.

### Q11. What is a synopsis in stream processing?
The notes mention **synopses** as compact summaries used with batch processing and sampling to improve efficiency. The PDF does not define a particular synopsis structure or give a separate synopsis algorithm.

### Q12. What is a blocking operator, and why is it a problem for an infinite stream?
A blocking operator cannot produce its first output until it has seen its entire input. The notes list **sorting, SUM, COUNT, MIN, MAX and AVG**. An infinite stream may never finish, so a blocking operator may never output a result. Apply it to a finite window to bound the input.

### Q13. Why are queries about past data difficult in a stream?
To save memory, processed items may be discarded. If a later query asks for an old period, the exact records may no longer be available. For example, if readings from 10:00–10:02 are discarded, a later request for their average cannot be answered exactly from the remaining readings alone. Retention, archival storage or suitable summaries can help.

## 1.4 Sampling data in a stream

### Q14. Why is sampling needed, and what are the six techniques in the notes?
Sampling selects a representative subset of a continuous stream for analysis, storage or decisions. It is useful for high-throughput streams, storage limitations and faster approximate analytics. The six techniques are:

1. **Random sampling:** Select items randomly; each item has an equal or known probability of selection. Example: retain 10% of events randomly.
2. **Reservoir sampling:** Maintains a fixed-size sample when stream length is unknown or potentially infinite. Example: keep five records in a reservoir and randomly replace records as new items arrive.
3. **Systematic sampling:** Selects at fixed intervals, e.g. every 10th record: 10, 20, 30, 40, … Easy and efficient, but a repeating pattern aligned with the interval can cause bias.
4. **Stratified sampling:** Divides data into categories/strata and samples from each, e.g. Electronics, Clothing, Grocery and Books. It helps represent each category.
5. **Weighted sampling:** Gives different selection probabilities to items. The notes give ₹500 (low weight), ₹5,000 (medium) and ₹5,00,000 (high) transactions as an example.
6. **Sliding-window sampling:** Samples only the recent portion of a stream, e.g. transactions from the last five minutes. Old data leaves as new data arrives.

### Q15. How do reservoir, systematic, stratified and weighted sampling differ?
- **Reservoir:** Fixed-size sample from an unknown-length stream.
- **Systematic:** Selects every kth record.
- **Stratified:** Selects from each category.
- **Weighted:** Selects items with probabilities based on assigned weights.

## 1.5 Decaying windows, counting ones and DGIM

### Q16. What is a decaying window, and how does it differ from a sliding window?
A sliding window removes old data once it falls outside the selected interval. A **decaying window** progressively reduces the importance of older data instead of simply removing it. The notes illustrate today = 100%, yesterday = 80%, two days ago = 60%, three days ago = 40%, four days ago = 20%. The exact decay function can vary.

Applications listed: recommendation systems, stock-market analysis, web-traffic analysis, fraud detection and social-media trends.

### Q17. What is “counting ones in a window”?
Given a binary stream such as `1011010011`, answer a question such as “How many 1s occurred in the last five positions?” Exact answers for different window sizes require retaining enough information about recent bits.

### Q18. What is DGIM, and what rules are given in the PDF?
**DGIM** stands for **Datar–Gionis–Indyk–Motwani**. It is introduced in the notes for counting ones in a recent window using compact buckets.

The four rules printed on Module 4 page 35 are:
1. There should be a `1` at the RHS in each and every bucket.
2. The size of every bucket should be \(2^n\), meaning it represents \(2^n\) ones.
3. Every bucket should have at least one `1`.
4. From right to left, every bucket should go in increasing order or be the same size as the previous bucket.

A bucket summarizes ones and tracks their recency, reducing the need to retain the full stream. **Source limitation:** the PDF lists these rules but does not provide a complete worked bucket-merging/query-estimation numerical, so none is invented here.

---

# 2. Module 5 — Finding Similar Items and Clustering

## 2.1 Distance measures

### Q19. What is a distance measure, and what are its four properties?
A distance measure quantifies how different two objects are. A small distance generally means greater similarity.

1. **Non-negativity:** \(d(A,B)\geq0\)
2. **Identity:** \(d(A,A)=0\)
3. **Symmetry:** \(d(A,B)=d(B,A)\)
4. **Triangle inequality:** \(d(A,C)\leq d(A,B)+d(B,C)\)

### Q20. Give the formulas and uses of the distance measures in the notes.

**Euclidean distance — numerical points**
\[
d(A,B)=\sqrt{(x_1-x_2)^2+(y_1-y_2)^2}
\]
It is the straight-line distance between two points.

**Jaccard similarity and distance — sets**
\[
J(A,B)=\frac{|A\cap B|}{|A\cup B|},\qquad d_J(A,B)=1-J(A,B)
\]

**Cosine comparison — vectors**
\[
\cos\theta=\frac{X\cdot Y}{\|X\|\|Y\|}
\]
The notes describe cosine distance as the angle between vectors: smaller angle means greater similarity. The angle ranges from 0° to 180°. The cosine similarity value itself is not the angle; use inverse cosine to calculate the angle.

**Edit distance — strings**
Counts edits needed to transform one string into another. The notes' classical example uses insertions and deletions; they also mention using the Longest Common Subsequence (LCS).

**Hamming distance — equal-length Boolean vectors**
Counts positions where the two vectors differ.

### Q21. Solve the Euclidean distance numerical.
Given \(A=(2,3)\), \(B=(5,7)\):
\[
d(A,B)=\sqrt{(2-5)^2+(3-7)^2}
=\sqrt{(-3)^2+(-4)^2}
=\sqrt{9+16}=\sqrt{25}=\boxed{5}
\]

### Q22. Solve the Jaccard numerical.
Given \(A=\{1,2,3,4\}\), \(B=\{2,4,5\}\):
- Intersection \(A\cap B=\{2,4\}\), size = 2.
- Union \(A\cup B=\{1,2,3,4,5\}\), size = 5.
\[
J(A,B)=\frac25=0.4,\qquad d_J=1-0.4=\boxed{0.6}
\]

### Q23. Solve the cosine numerical.
Given \(X=[1,2,-1]\), \(Y=[2,1,1]\).

**Dot product**
\[
X\cdot Y=(1)(2)+(2)(1)+(-1)(1)=2+2-1=3
\]

**L2 norms**
\[
\|X\|=\sqrt{1^2+2^2+(-1)^2}=\sqrt6
\]
\[
\|Y\|=\sqrt{2^2+1^2+1^2}=\sqrt6
\]

**Cosine and angle**
\[
\cos\theta=\frac{3}{\sqrt6\sqrt6}=\frac36=\frac12
\]
\[
\theta=\cos^{-1}(1/2)=\boxed{60^\circ}
\]
Therefore cosine similarity = 0.5 and angle = 60°.

### Q24. Solve edit distance using the classical method.
Transform `ABCDE` into `ACFDEG`:
1. Delete `B`: `ACDE`.
2. Insert `F` at position 3: `ACFDE`.
3. Insert `G` at position 6: `ACFDEG`.

Total = one deletion + two insertions = **3 operations**. The notes also give `good` → `goodbye` as edit distance 3 because `bye` is inserted.

### Q25. How is edit distance calculated using LCS in the numerical PDF?
The numerical PDF uses:
\[
d(X,Y)=|X|+|Y|-2\,LCS(X,Y)
\]
For \(X=\texttt{ABCDE}\) and \(Y=\texttt{ACFDEG}\):
- \(|X|=5\), \(|Y|=6\).
- One LCS is `ACDE`, length = 4.
\[
d(X,Y)=5+6-2(4)=11-8=\boxed{3}
\]

### Q26. Solve the Hamming distance example.
Given:
```text
P1 = 1 0 1 0 1
P2 = 1 1 1 1 0
```
Compare positions: same, different, same, different, different. There are three mismatches, so **Hamming distance = 3**.

## 2.2 CURE algorithm

### Q27. What is CURE, and why is it used?
**CURE** stands for **Clustering Using Representatives**. It is a hierarchical-based clustering technique that lies between centroid-based and all-point approaches. Hierarchical clustering starts with each point as a separate cluster and repeatedly merges clusters.

CURE is useful for arbitrary-shaped, including non-spherical, clusters. It uses several representative points rather than only one centroid, and the notes describe it as useful for handling outliers.

### Q28. Explain the CURE algorithm and its flow diagram.
The flow shown on Module 5 page 18 is:

1. Draw a random sample of size \(s\) from the dataset.
2. Divide the sample into \(p\) partitions.
3. Cluster each partition independently to create intermediate/partial clusters.
4. Identify and remove outliers.
5. Merge and cluster the partial clusters to form final clusters.
6. Assign data points not included in the sample to the nearest cluster using the closest representative points.

Within a cluster, CURE:
- Selects multiple representative points.
- Uses them to capture the cluster shape.
- Shrinks the representatives toward the cluster centroid.
- Iteratively merges the closest clusters.

**Source limitation:** The PDFs show the shrinking concept but do not provide a complete numerical value for the shrinking factor or exact representative coordinates.

### Q29. Why does CURE use representative points instead of only the centroid?
A single centroid may not describe an arbitrary or non-spherical cluster well. Multiple representatives capture more of the cluster's shape. Shrinking them toward the centroid reduces their extremity while retaining shape information.

## 2.3 Stream clustering

### Q30. What is stream clustering, and how do buckets work?
Stream clustering groups continuously arriving data points without retaining the entire history. It maintains compact summaries in **buckets**.

A bucket may store:
- Number of points.
- Mean/centroid.
- Timestamp.
- Other summary information.

**Initialization example:** Bucket 1 = 10 points, Bucket 2 = 20 points, Bucket 3 = 15 points.

### Q31. Solve the bucket-merging example.
Before merging:
- B1 = 10 points
- B2 = 12 points
- B3 = 20 points

Merge B1 and B2:
\[
10+12=22
\]
After merging: B1+B2 = 22 points and B3 = 20 points. Fewer buckets reduce storage needs.

### Q32. What queries can stream-clustering summaries answer?
The notes list:
- What are the current clusters?
- How many points are in a cluster?
- What is the approximate centroid?
- Which cluster does a new point belong to?

The bucket summaries are used instead of processing the entire historical stream again.

---

# 3. CURE Customer-Clustering Case Study

The separate CURE PDF gives the following food-delivery customer data:

| Customer | Monthly orders | Average order value | Delivery distance | Monthly spending |
|---|---:|---:|---:|---:|
| C1 | 10 | 300 | 3 | 3000 |
| C2 | 12 | 320 | 4 | 3800 |
| C3 | 9 | 280 | 3 | 2500 |
| C4 | 30 | 700 | 8 | 21000 |
| C5 | 32 | 750 | 9 | 24000 |
| C6 | 11 | 310 | 4 | 3400 |
| C7 | 2 | 2000 | 25 | 4000 |

### Q33. Why must the customer data be scaled before comparing distances?
The features use different numerical ranges. Monthly spending is in the thousands, while order count is in tens and delivery distance is much smaller. Without scaling, large-scale features can dominate distance calculations. The PDF explicitly says to scale the data before identifying similar customers.

### Q34. State the final customer clusters and explain C7.
- **Cluster 1 — Regular Customers:** `{C1, C2, C3, C6}`. Similar numbers of orders, order values and spending. Suggested plan: regular/standard customer plan.
- **Cluster 2 — Premium Customers:** `{C4, C5}`. High order frequency, average order value and monthly spending. Suggested plan: premium membership or loyalty plan.
- **Cluster 3 — Outlier:** `{C7}`. Only two orders, very high average order value (2000) and large delivery distance (25).

An outlier is unusual, but the data does **not** prove that C7 is fraudulent. The PDF does not provide all scaled values, exact inter-customer distances or representative-point coordinates, so a complete numeric CURE trace cannot be reproduced from the source.

---

# 4. Module 4 & 5 Numericals

## 4.1 Bloom Filter

### Q35. What is the Bloom Filter numerical in the uploaded numerical PDF asking?
Insert 10 and 7 into a Bloom Filter of size \(m=5\), using:
\[
h_1(x)=x\bmod5,\qquad h_2(x)=(2x+6)\bmod5
\]
Then check whether 14 and 15 are present.

### Q36. Solve the Bloom Filter insertion step by step.
Assume five bit positions indexed 0–4 and all bits initially 0.

**Insert 10**
\[
h_1(10)=10\bmod5=0
\]
\[
h_2(10)=(2(10)+6)\bmod5=26\bmod5=1
\]
Set bit positions 0 and 1 to 1.

**Insert 7**
\[
h_1(7)=7\bmod5=2
\]
\[
h_2(7)=(2(7)+6)\bmod5=20\bmod5=0
\]
Set bit positions 2 and 0 to 1.

Final bit array, indexed 0 to 4:
```text
Index:  0 1 2 3 4
Bits:   1 1 1 0 0
```

### Q37. Check 14 and 15 in the Bloom Filter.
**Check 14**
\[
h_1(14)=14\bmod5=4
\]
\[
h_2(14)=(2(14)+6)\bmod5=34\bmod5=4
\]
Bit at index 4 is 0. Therefore **14 is definitely absent** according to this Bloom Filter.

**Check 15**
\[
h_1(15)=15\bmod5=0
\]
\[
h_2(15)=(2(15)+6)\bmod5=36\bmod5=1
\]
Bits at indices 0 and 1 are both 1, so the filter reports that 15 **may be present**. But 15 was never inserted. This is a **false positive**.

### Q38. What is a Bloom Filter false positive?
A false positive occurs when the filter reports that an item may be present even though it was not inserted. A Bloom Filter can indicate possible presence, not guarantee presence. If any required bit is 0, the item is definitely absent; if all required bits are 1, it may be present.

## 4.2 Decaying-window aggregate sum

### Q39. Solve the decaying-window numerical from the notes.
Stream:
```text
AI, ML, AI, ML, ML, ML, AI
```
Initial weight = 1; decay factor = 0.1. As in the handwritten numerical, position 7 is the newest item and has weight \(0.1^0=1\); moving backward, weights are multiplied by 0.1.

| Position | Item | Decayed weight |
|---:|---|---:|
| 1 | AI | \(0.1^6=0.000001\) |
| 2 | ML | \(0.1^5=0.00001\) |
| 3 | AI | \(0.1^4=0.0001\) |
| 4 | ML | \(0.1^3=0.001\) |
| 5 | ML | \(0.1^2=0.01\) |
| 6 | ML | \(0.1^1=0.1\) |
| 7 | AI | \(0.1^0=1\) |

**Aggregate sum for AI** (positions 1, 3 and 7):
\[
S_{AI}=0.000001+0.0001+1=\boxed{1.000101}
\]

**Aggregate sum for ML** (positions 2, 4, 5 and 6):
\[
S_{ML}=0.00001+0.001+0.01+0.1=\boxed{0.11101}
\]

AI has the larger decayed aggregate because it appears at the newest position, which has the greatest weight.

---

# 5. Module 6 — Real-Time Big Data Models

## 5.1 PageRank Algorithm

### Q40. What is PageRank, and what principle does it use?
PageRank was developed by Larry Page and Sergey Brin at Google to rank webpages by importance and relevance. A page may be important when many other pages link to it, especially when those links come from important pages.

### Q41. What are the advantages and applications of PageRank?
**Advantages from the notes:**
- Considers both the number and quality of incoming links.
- Helps improve search-result ranking.
- Links from reputable pages can contribute more than links from less important pages, although manipulation is still possible.
- Uses relationships between pages rather than page content alone.

**Applications:** Search engines, website analysis, social-network influence, citation networks and recommendation systems.

**Source limitation:** The uploaded Module 6 PDF explains PageRank conceptually but does not provide its mathematical formula, damping factor or a worked PageRank numerical. These are not fabricated in this guide.

## 5.2 Collaborative Filtering

### Q42. What is Collaborative Filtering, and what are its types?
**Collaborative Filtering (CF)** recommends items using other users' preferences, ratings or behaviour. It assumes that users who liked similar items in the past may like similar items in the future.

- **User-based CF:** Finds users with similar interests and recommends items they liked.
- **Item-based CF:** Finds items that receive similar ratings from users and recommends items similar to those the target user already likes.

**Working steps:** (1) collect ratings, (2) calculate similarity, (3) predict ratings for unrated items, (4) rank and recommend the top items.

**Advantages:** Personalization, discovery of new items, no requirement for item descriptions, and potentially improved engagement.

**Applications:** Spotify/music, Netflix/video, Amazon/e-commerce, social media and online learning.

## 5.3 Collaborative Filtering — worked numerical

### Q43. How are recommendations generated for User A in the PDF example?
Users rate movies from 1 to 5. A dash means User A has not rated that movie.

| User | Movie 1 | Movie 2 | Movie 3 | Movie 4 | Movie 5 | Movie 6 | Movie 7 |
|---|---:|---:|---:|---:|---:|---:|---:|
| A | 5 | 4 | — | — | — | — | — |
| B | 5 | 4 | 5 | 4 | 5 | 4 | 3 |
| C | 4 | 5 | 4 | 5 | 4 | 5 | 4 |
| D | 1 | 2 | 2 | 1 | 2 | 1 | 2 |

**Step 1 — Compare only co-rated movies.** User A has rated Movie 1 and Movie 2, so the vectors are A = (5, 4), B = (5, 4), C = (4, 5), D = (1, 2).

**Step 2 — Calculate cosine similarity.**
\[
\operatorname{sim}(A,B)=\frac{A\cdot B}{\|A\|\|B\|}
\]

For A and B:
\[
\frac{(5)(5)+(4)(4)}{\sqrt{5^2+4^2}\sqrt{5^2+4^2}}
=\frac{41}{41}=\boxed{1.000}
\]

For A and C:
\[
\frac{(5)(4)+(4)(5)}{\sqrt{41}\sqrt{41}}
=\frac{40}{41}\approx\boxed{0.976}
\]

For A and D:
\[
\frac{(5)(1)+(4)(2)}{\sqrt{41}\sqrt{1^2+2^2}}
=\frac{13}{\sqrt{41}\sqrt5}\approx\boxed{0.908}
\]

The two nearest neighbours selected in the notes are **User B (1.000)** and **User C (0.976)**.

**Step 3 — Predict an unrated movie using a similarity-weighted average.**
\[
\widehat R_{A,m}=
\frac{\sum_u \operatorname{sim}(A,u)\,R_{u,m}}
{\sum_u \operatorname{sim}(A,u)}
\]

For Movie 3, B rated it 5 and C rated it 4:
\[
\widehat R_{A,3}=
\frac{(1.000)(5)+(0.976)(4)}{1.000+0.976}
=\frac{8.904}{1.976}\approx\boxed{4.51}
\]

**Step 4 — Predicted ratings from the source:**

| Movie | User B | User C | Predicted rating |
|---|---:|---:|---:|
| Movie 3 | 5 | 4 | 4.51 |
| Movie 4 | 4 | 5 | 4.49 |
| Movie 5 | 5 | 4 | 4.51 |
| Movie 6 | 4 | 5 | 4.49 |
| Movie 7 | 3 | 4 | 3.49 |

**Step 5 — Top-five recommendations:**
1. Movie 3 — 4.51
2. Movie 5 — 4.51
3. Movie 4 — 4.49
4. Movie 6 — 4.49
5. Movie 7 — 3.49

Equal predicted ratings may be listed in either order, as noted in the source.

## 5.4 Networks as Graphs

### Q44. How is a social network represented as a graph?
A graph contains:
- **Vertex/node:** A person, organization or entity.
- **Edge/link:** A relationship between entities.
- **Directed graph:** Relationships have a direction, e.g. following someone on Instagram.
- **Undirected graph:** Relationships have no direction, e.g. a Facebook friendship.
- **Weighted graph:** Edges have weights representing relationship strength, frequency or interaction count.

Applications listed include identifying influential people, recommending friends, detecting communities and analyzing information flow.

### Q45. What is clustering of social-network graphs?
It groups nodes according to connections or similarities. Nodes with frequent interactions or many shared connections may belong to the same cluster. The notes' example has nine students in three dense groups: A–B–C (Computer Science), D–E–F (IT) and G–H–I (Electronics). Dense within-group connections and sparse between-group connections indicate natural communities.

## 5.5 Girvan–Newman Algorithm

### Q46. What is the Girvan–Newman algorithm?
It is a community-detection algorithm used to identify groups of closely connected nodes in a network. It finds edges that act as bridges between communities using **edge betweenness**.

### Q47. State the steps of Girvan–Newman.
1. **Calculate edge betweenness:** Count how many shortest paths between node pairs pass through each edge.
2. **Find the edge with the highest betweenness:** It is a candidate bridge between groups.
3. **Remove that edge.**
4. **Recalculate edge betweenness:** Removing an edge can change shortest paths.
5. **Repeat** until the desired communities form.

**Advantages:** Identifies communities, detects bridge connections and helps reveal network structure.

**Applications:** Social networks, research networks, online communities and communication networks.

**Source limitation:** Module 6 gives the algorithm and a link to a tutorial, but does not include the graph and full numerical calculation itself. The steps above are source-based; no fabricated edge-betweenness numerical is presented.

---

# 6. High-value comparison questions

### Q48. Compare the main distance measures.
| Measure | Data type / idea | What is calculated? |
|---|---|---|
| Euclidean | Numerical points | Straight-line distance |
| Jaccard | Sets | One minus intersection/union |
| Cosine | Vectors | Angle/directional similarity |
| Edit distance | Strings | Number of edits needed to transform one string |
| Hamming | Equal-length Boolean vectors | Number of mismatched positions |

### Q49. Compare CURE and stream clustering.
- **CURE:** Hierarchical clustering, uses multiple representative points, supports arbitrary-shaped clusters and handles outliers.
- **Stream clustering:** Handles continuously arriving points and maintains compact bucket summaries to control memory use.

### Q50. Compare sliding and decaying windows.
- **Sliding window:** Removes old records outside the selected interval.
- **Decaying window:** Keeps older records' influence but reduces their weights over time.

### Q51. Compare PageRank and Collaborative Filtering.
- **PageRank:** Estimates importance from link relationships in a graph.
- **Collaborative Filtering:** Recommends items based on similar users' or items' ratings/behaviour.

### Q52. What is the difference between an outlier and a community?
An **outlier** is a data point with unusual characteristics compared with the other points, as C7 is in the CURE example. A **community** is a group of graph nodes with stronger or denser connections within the group than between groups.

---

# 7. Last-minute formula and algorithm sheet

## Formulas
| Concept | Formula |
|---|---|
| Euclidean distance (2D) | \(d=\sqrt{(x_1-x_2)^2+(y_1-y_2)^2}\) |
| Jaccard similarity | \(J=|A\cap B|/|A\cup B|\) |
| Jaccard distance | \(d_J=1-J\) |
| Cosine similarity | \(\cos\theta=(X\cdot Y)/(\|X\|\|Y\|)\) |
| LCS-based edit distance used in numerical PDF | \(d(X,Y)=|X|+|Y|-2LCS(X,Y)\) |
| User-based CF predicted rating | \(\widehat R_{A,m}=\sum_u sim(A,u)R_{u,m}/\sum_u sim(A,u)\) |
| Bloom Filter hash functions in numerical | \(h_1(x)=x\bmod5\), \(h_2(x)=(2x+6)\bmod5\) |
| Decay weight in numerical | \(0.1^{\text{age}}\), with newest item age 0 |

## Algorithm steps to memorize
- **DSMS:** Source → ingestion → processing → storage → querying → visualization/reporting → integration.
- **CURE:** Random sample → partition → cluster partitions → eliminate outliers → merge partial clusters → assign non-sampled points to nearest representative-based cluster.
- **Reservoir sampling:** Maintain fixed-size reservoir; randomly replace entries as new records arrive.
- **Girvan–Newman:** Calculate edge betweenness → remove highest-betweenness edge → recalculate → repeat.
- **User-based CF:** Collect ratings → calculate similarity → choose nearest neighbours → predict unrated items → rank recommendations.
- **DGIM:** Memorize the four rules exactly as printed in Module 4 page 35.

## Answers to remember
- Euclidean distance for (2,3) and (5,7): **5**
- Jaccard similarity/distance: **0.4 / 0.6**
- Cosine angle for the provided vectors: **60°**
- Edit distance `ABCDE` → `ACFDEG`: **3**
- Hamming distance `10101` vs `11110`: **3**
- CURE clusters: Regular `{C1,C2,C3,C6}`, Premium `{C4,C5}`, Outlier `{C7}`
- Bloom Filter after inserting 10 and 7: bits `[1,1,1,0,0]` for indices 0–4
- Bloom Filter check: 14 is absent; 15 produces a false positive
- Decayed sum: AI = **1.000101**, ML = **0.11101**
- Collaborative filtering neighbours for A: B = **1.000**, C = **0.976**; top movies are 3, 5, 4, 6, 7
- Stream-cluster merge: 10 + 12 = **22 points**

---

# Source coverage and notes

This guide consolidates the five uploaded PDFs:
1. **Module 4 — Mining Data Streams** (35 pages): stream model, DSMS architecture, queries, windows, sources, processing issues, sampling, decaying windows, counting ones and DGIM.
2. **Module 5 — Finding Similar Items and Clustering** (24 pages): distance properties/measures, CURE and stream clustering.
3. **CURE Algorithm** (4 pages): food-delivery customer dataset, scaling, groups and outlier.
4. **Module 4 and 5 — Numericals** (3 pages): Bloom Filter insertion/membership check, LCS-based edit distance and decaying-window aggregate sums.
5. **Module 6 — Real-Time Big Data Models** (10 pages): PageRank, Collaborative Filtering and worked movie-recommendation numerical, graph networks, social-network clustering and Girvan–Newman.

**Important source limitations:** The PDFs do not provide a complete DGIM bucket-merging numerical, a complete PageRank formula/numerical, exact scaled values and representative-point calculations for the CURE customer example, or a worked edge-betweenness graph for Girvan–Newman. These are flagged instead of being filled with unsupported calculations.
