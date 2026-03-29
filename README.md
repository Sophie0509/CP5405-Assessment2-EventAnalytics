# CP5405 Assessment 2 – Event Analytics Platform (MongoDB)

## Project Overview
This project implements an event analytics platform using MongoDB. The system ingests multiple event data streams including ticket scans, RFID movement tracking, and attendee feedback. The database is designed using MongoDB schema design best practices, and the system supports analytics, aggregation queries, and performance optimisation.

The project demonstrates:
- Data ingestion from CSV files into MongoDB
- Schema design using referenced collections
- Aggregation pipelines for analytics
- Index optimisation and performance benchmarking
- Bulk operations and TTL indexes
- Performance comparison and scalability discussion

---

## System Architecture and Schema Design
The database is named **event_platform** and includes the following collections:

| Collection | Description |
|------------|-------------|
| ticket_scans | Ticket entry scan records |
| rfid_movements | RFID location tracking data |
| feedback | Attendee feedback and ratings |
| attendees_summary | Denormalised analytics summary |
| performance_cache | Temporary cache data (TTL index) |

### Schema Design Approach
A **referenced schema design** was used for raw event streams because each attendee can generate many ticket scans, RFID movements, and feedback records. Embedding these records would cause documents to grow rapidly and reduce scalability.

Therefore:
- Raw event streams are stored in separate collections
- Collections are linked using **attendee_id**
- A **denormalised summary collection (attendees_summary)** is used for analytics queries

This design follows MongoDB best practices for **high-cardinality event data** and supports horizontal scalability.

---

## Data Ingestion
CSV files are ingested into MongoDB using a Python script.

Files:
- ticket_scans.csv
- rfid_movements.csv
- feedback.csv

Run the ingestion script:
python ingest_sample_data.py

This script inserts data into MongoDB collections and converts timestamps into datetime format.

---

## Indexing Strategy
Several indexes were implemented to improve query performance and scalability.

### Indexes Implemented
| Index Type | Fields | Purpose |
|-------------|--------|---------|
| Compound Index | attendee_id + timestamp | Optimise timeline queries |
| Unique Index | ticket_id | Prevent duplicate ticket records |
| TTL Index | expireAt | Automatically remove temporary cache data |

### ESR Rule
The compound index follows the **ESR rule (Equality, Sort, Range)**:
- Equality → attendee_id
- Sort → timestamp
- Range → timestamp

This significantly reduces the number of documents examined during queries.

---

## Aggregation Pipelines
Three aggregation pipelines were implemented for analytics:

### Pipeline 1 – Engagement vs Feedback
Combines RFID movement counts with feedback ratings to analyse whether highly active attendees give higher ratings.

### Pipeline 2 – Time-based Entry Trends
Analyzes ticket scan timestamps to identify peak event entry hours.

### Pipeline 3 – Mobility vs Sentiment Analysis
Combines movement data and feedback sentiment to identify patterns between attendee mobility and satisfaction.

These pipelines demonstrate:
- `$lookup`
- `$group`
- `$project`
- `$sort`
- `$addFields`
- Multi-collection joins

---

## Performance Optimisation
Several performance optimisation techniques were implemented:

| Optimisation | Purpose |
|---------------|---------|
| Compound Index | Reduce collection scans |
| Projection Optimisation | Reduce document size |
| Bulk Operations | Improve write performance |
| TTL Index | Automatic cleanup of temporary data |
| Explain() | Query performance analysis |
| Index Performance Comparison | Benchmark different index strategies |

Performance was evaluated using: 
This script inserts data into MongoDB collections and converts timestamps into datetime format.

---

## Indexing Strategy
Several indexes were implemented to improve query performance and scalability.

### Indexes Implemented
| Index Type | Fields | Purpose |
|-------------|--------|---------|
| Compound Index | attendee_id + timestamp | Optimise timeline queries |
| Unique Index | ticket_id | Prevent duplicate ticket records |
| TTL Index | expireAt | Automatically remove temporary cache data |

### ESR Rule
The compound index follows the **ESR rule (Equality, Sort, Range)**:
- Equality → attendee_id
- Sort → timestamp
- Range → timestamp

This significantly reduces the number of documents examined during queries.

---

## Aggregation Pipelines
Three aggregation pipelines were implemented for analytics:

### Pipeline 1 – Engagement vs Feedback
Combines RFID movement counts with feedback ratings to analyse whether highly active attendees give higher ratings.

### Pipeline 2 – Time-based Entry Trends
Analyzes ticket scan timestamps to identify peak event entry hours.

### Pipeline 3 – Mobility vs Sentiment Analysis
Combines movement data and feedback sentiment to identify patterns between attendee mobility and satisfaction.

These pipelines demonstrate:
- `$lookup`
- `$group`
- `$project`
- `$sort`
- `$addFields`
- Multi-collection joins

---

## Performance Optimisation
Several performance optimisation techniques were implemented:

| Optimisation | Purpose |
|---------------|---------|
| Compound Index | Reduce collection scans |
| Projection Optimisation | Reduce document size |
| Bulk Operations | Improve write performance |
| TTL Index | Automatic cleanup of temporary data |
| Explain() | Query performance analysis |
| Index Performance Comparison | Benchmark different index strategies |

Performance was evaluated using:.explain("executionStats")

Metrics compared:
- totalDocsExamined
- totalKeysExamined
- executionTimeMillis

A performance comparison chart was generated to compare:
- No index
- Single index
- Compound index
- Optimal ESR index

---

## Scalability Discussion
This system is designed to scale to large datasets by:
- Separating write-heavy event streams from read-heavy analytics collections
- Using referenced collections for high-cardinality data
- Using denormalised summary collections for fast analytics queries
- Implementing compound indexes for efficient query filtering
- Using TTL indexes for temporary data cleanup
- Using bulk operations for high-volume inserts

This architecture supports horizontal scalability and is suitable for large event analytics platforms.

---

## Setup Instructions
1. Install Python
2. Install required packages:
3. Create MongoDB Atlas cluster
4. Create database: **event_platform**
5. Update MongoDB connection string in `ingest_sample_data.py`
6. Run ingestion script:
7. Run aggregation and performance scripts in Jupyter Notebook / Colab

---

## Author
Zifei Yu – Event Analytics MongoDB Project
