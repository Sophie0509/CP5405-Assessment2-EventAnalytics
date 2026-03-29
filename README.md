# CP5405 Assessment 2 – Event Data Platform (MongoDB)

## 1. Project Overview
This project implements a MongoDB-based event analytics platform that ingests ticket scans, RFID movement data, and attendee feedback data. The system supports data ingestion, schema design, indexing strategies, aggregation analytics, and performance optimisation.

The objective of this project is to demonstrate MongoDB data modelling, aggregation pipelines, indexing optimisation, and scalability considerations for large datasets. The platform simulates an event environment where attendees enter an event using tickets, move around the venue using RFID tracking, and submit feedback. The system stores, processes, and analyses this data using MongoDB.

---

## 2. Setup Instructions

### Requirements
The following software is required:
- Python 3.x
- MongoDB Atlas or Local MongoDB
- Python libraries:
  - pymongo
  - pandas
  - matplotlib

### Install Dependencies
```bash
pip install pymongo pandas matplotlib
```

### Import CSV Data into MongoDB
Run the ingestion script:
```bash
python ingest_sample_data.py
```

This script will:
- Create MongoDB collections
- Import CSV data
- Generate summary collection
- Create indexes

### Run Aggregation Pipelines
```bash
python aggregation_pipelines.py
```

### Run Performance Optimisation Tests
```bash
python performance_optimisation.py
```

---

## 3. Database Schema Design

The database uses a referenced schema design combined with a summary collection.

### Collections
1. **ticket_scans**
   - ticket_id
   - attendee_id
   - event_id
   - scan_time
   - gate

2. **rfid_movements**
   - attendee_id
   - location_id
   - timestamp
   - rfid_tag

3. **feedback**
   - attendee_id
   - rating
   - sentiment
   - comments
   - timestamp

4. **attendees_summary**
   - attendee_id
   - total_scans
   - locations_visited
   - avg_rating
   - sentiment
   - engagement_score

5. **performance_cache**
   - cache_type
   - createdAt
   - expireAt

### Schema Design Strategy
- Referenced schema used for operational collections (ticket_scans, rfid_movements, feedback)
- Summary collection used for analytics and performance optimisation
- This reduces aggregation cost and improves analytics performance
- The summary collection avoids repeated aggregation queries on large datasets

---

## 4. Indexing Strategies and Optimisation

The following indexes were implemented:

| Index Type | Fields | Purpose |
|------------|--------|---------|
| Compound Index | attendee_id, timestamp | Optimise timeline queries |
| Unique Index | ticket_id | Prevent duplicate ticket scans |
| TTL Index | expireAt | Automatically delete temporary cache data |
| Single Index | timestamp | Time-based queries |
| Single Index | attendee_id | Attendee filtering |

### ESR Rule
The compound index `(attendee_id, timestamp)` follows the ESR rule:
- Equality → attendee_id
- Sort → timestamp
- Range → timestamp

This significantly reduces `totalDocsExamined` and improves query performance.

### Performance Improvement
Before optimisation:
- Collection Scan (COLLSCAN)
- Large number of documents examined
- In-memory sorting required

After optimisation:
- Index Scan (IXSCAN)
- Significant reduction in documents examined
- Sorting handled by index
- Faster execution time
- Reduced memory usage

---

## 5. Aggregation Analytics

Three aggregation pipelines were implemented.

### Pipeline 1 – Engagement vs Feedback
Joins ticket scans and feedback collections to analyse engagement score vs feedback rating.

### Pipeline 2 – Time-based Entry Trends
Analyses entry patterns by hour to identify peak entry times.

### Pipeline 3 – Mobility vs Sentiment Analysis
Analyses attendee movement patterns and compares with sentiment ratings to identify behavioural patterns.

These pipelines demonstrate:
- Multi-collection joins (`$lookup`)
- Grouping and aggregation (`$group`)
- Sorting and filtering
- Time-based analytics
- Behaviour analytics

---

## 6. Performance Optimisation

Several performance optimisation techniques were implemented:

1. Compound Index Optimisation
2. Query Filtering
3. Projection Optimisation
4. Bulk Write Operations
5. TTL Index for temporary data
6. Explain Plan Performance Analysis
7. Index Performance Comparison Chart

### Projection Optimisation
Projection reduces document size and improves query performance by returning only required fields.

### Bulk Operations
Bulk write operations improve insert/update performance when processing large datasets.

### TTL Index
TTL index automatically deletes temporary cache data after expiration time, reducing storage usage.

---

## 7. Scalability Discussion

If the dataset increases by 100x, the system can scale by:

- Using compound indexes to reduce query cost
- Using aggregation pipelines instead of client-side processing
- Using summary collections to reduce repeated aggregations
- Using bulk writes for high-volume inserts
- Using TTL indexes for temporary data cleanup
- Implementing sharding for very large datasets
- Using projection to reduce document size
- Using indexed queries instead of collection scans

These techniques follow professional MongoDB performance optimisation practices.

---

## 8. Critical Reflection

MongoDB is suitable for event analytics systems because of its flexible schema, aggregation framework, and indexing capabilities. The aggregation framework allows complex analytics to be performed directly in the database, reducing application processing time.

However, aggregation pipelines can become expensive without proper indexing. Index design is critical for performance optimisation. Compound indexes following the ESR rule significantly improve query performance.

For very large datasets, sharding and distributed clusters would be required to maintain performance and scalability. Overall, this project demonstrates how MongoDB can be used for data ingestion, analytics, and performance optimisation in event data platforms.

---

## 9. Conclusion

This project demonstrates MongoDB data ingestion, schema design, aggregation analytics, indexing strategies, and performance optimisation techniques. The system is designed to support large-scale event analytics data and demonstrates scalability, performance optimisation, and database design best practices.
