# 2. Types and Characteristics of Big Data

## The 5 Vs of Big Data

1. **Volume:** Amount of data. Example: billions of social-media posts.
2. **Velocity:** Speed at which data is generated, transmitted, or processed. Example: payment events arriving continuously.
3. **Variety:** Different formats and sources. Example: tables, JSON, audio, images, and video.
4. **Veracity:** Data quality and trustworthiness. Example: removing duplicates and fake reviews.
5. **Value:** Useful insight obtained from data. Example: improving recommendations from customer behaviour.

### Quick identification
- Huge quantity → Volume
- Fast arrival → Velocity
- Many formats → Variety
- Inaccurate or incomplete records → Veracity
- Business benefit or useful insight → Value

## Types of Big Data

### Structured data
Data arranged according to a predefined schema, commonly in rows and columns.

**Examples:** SQL tables, student marks, employee records, and transaction tables.

**Typical tools:** Relational database management systems (RDBMS), SQL, and data marts.

### Unstructured data
Data without a fixed tabular schema.

**Examples:** Images, videos, audio recordings, CCTV footage, free-text reviews, and social-media posts.

**Analysis methods:** Natural language processing, computer vision, and specialised analytics.

### Semi-structured data
Data that does not require a rigid relational table schema but contains keys, tags, or metadata.

**Examples:** JSON, XML, HTML, and many application logs.

JSON example:

```json
{
  "name": "Gayatri",
  "department": "CSE",
  "year": 3
}
```

### Hybrid data
A combination of different data types used together.

**Example:** An e-commerce company combines purchase tables, website logs, reviews, and product images to improve recommendations.

## Comparison

| Property | Structured | Semi-structured | Unstructured |
|---|---|---|---|
| Organisation | Fixed rows/columns | Keys, tags, metadata | No fixed tabular structure |
| Schema | Predefined | Flexible or partially defined | No predefined tabular schema |
| Examples | SQL tables | JSON, XML, HTML | Images, audio, video |
| Typical processing | SQL queries | Parsing and querying fields | NLP, vision, specialised analytics |

## Case-study classifications
- Quiz scores → Structured
- Recorded lectures → Unstructured
- Feedback stored as JSON → Semi-structured
- Bank transaction fields → Structured
- Customer-call recordings → Unstructured
- XML login logs → Semi-structured

**Note:** These classifications describe common cases. A file's classification can depend on how it is represented and used.
