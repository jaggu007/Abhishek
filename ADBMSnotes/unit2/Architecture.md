# Overview of MongoDB Architecture

MongoDB follows a **document-oriented architecture** where data is stored as flexible JSON-like documents (BSON format) instead of rows and columns. It is designed for  **high performance, scalability, and availability** .

## MongoDB Architecture Diagram

```text
+---------------------+
|   Client Application|
| (Web/Mobile/Desktop)|
+----------+----------+
           |
           v
+---------------------+
| MongoDB Driver      |
| (Java, Python, etc.)|
+----------+----------+
           |
           v
+---------------------+
| MongoDB Server      |
|      (mongod)       |
+----------+----------+
           |
   -----------------
   |       |       |
   v       v       v
Database Collection Documents
```

---

# Main Components

## 1. Client Application

The client application sends requests to MongoDB.

Examples:

* Java Application
* Python Application
* Node.js Application
* MongoDB Compass

Example:

```javascript
db.students.find()
```

---

## 2. MongoDB Driver

A driver acts as a bridge between the application and MongoDB.

Examples:

* Java Driver
* Python Driver (PyMongo)
* Node.js Driver
* C# Driver

Flow:

```text
Application
     ↓
MongoDB Driver
     ↓
MongoDB Server
```

---

## 3. MongoDB Server (mongod)

The `mongod` process is the core database server.

Responsibilities:

* Stores data
* Processes queries
* Handles indexing
* Manages authentication
* Controls replication
* Supports sharding

Check if the server is running:

```bash
mongosh
```

---

# Data Storage Hierarchy

MongoDB organizes data in four levels.

```text
Database
   ↓
Collection
   ↓
Document
   ↓
Fields
```

---

## Database

A database contains multiple collections.

Example:

```javascript
use KIET
```

Databases:

```text
KIET
admin
config
local
```

---

## Collection

A collection is similar to a table in relational databases.

Example:

```javascript
db.createCollection("students")
```

Collection:

```text
students
```

---

## Document

A document is similar to a row.

Example:

```javascript
{
  "name": "Abhishek",
  "branch": "CSE",
  "year": 2
}
```

Documents are stored in BSON format.

---

## Fields

Fields are similar to columns.

Example:

```javascript
{
  "name": "Abhishek",
  "branch": "CSE",
  "year": 2
}
```

Fields:

```text
name
branch
year
```

---

# Storage Engine

MongoDB uses the **WiredTiger Storage Engine** by default.

Features:

* Compression
* Concurrency Control
* Caching
* Crash Recovery

```text
Application
      ↓
MongoDB Server
      ↓
WiredTiger Engine
      ↓
Disk Storage
```

---

# Indexing Layer

Indexes improve query performance.

Without Index:

```text
Collection Scan
```

With Index:

```text
Index Lookup
```

Create Index:

```javascript
db.students.createIndex({name:1})
```

Benefits:

* Faster search
* Faster sorting
* Reduced query time

---

# Replication Architecture

Replication provides high availability.

```text
          Primary
         /       \
        /         \
 Secondary     Secondary
```

### Primary Node

* Accepts read/write operations

### Secondary Nodes

* Copy data from primary
* Used for failover

Example Replica Set:

```text
rs0
 ├── Primary
 ├── Secondary
 └── Secondary
```

If the primary fails:

```text
Automatic Election
```

A secondary becomes the new primary.

---

# Sharding Architecture

Used for horizontal scaling.

```text
                 Client
                    |
                    v
              Mongo Router
                 (mongos)
                    |
    ---------------------------------
    |               |               |
    v               v               v
 Shard 1         Shard 2         Shard 3
```

Each shard stores part of the data.

Example:

| Student IDs  | Shard   |
| ------------ | ------- |
| 1–10000     | Shard 1 |
| 10001–20000 | Shard 2 |
| 20001–30000 | Shard 3 |

Benefits:

* Handles huge datasets
* Supports millions of records
* Improves performance

---

# Configuration Servers

Configuration servers maintain metadata about shards.

```text
Config Server
      ↓
Stores:
- Shard locations
- Chunk information
- Cluster metadata
```

---

# Query Processing Architecture

Suppose the query is:

```javascript
db.students.find({branch:"CSE"})
```

Flow:

```text
User Query
    ↓
MongoDB Driver
    ↓
Query Optimizer
    ↓
Index Lookup
    ↓
Collection Scan (if needed)
    ↓
Result Returned
```

---

# MongoDB Deployment Models

## Standalone

```text
Client
  ↓
MongoDB Server
```

Used for:

* Learning
* Development
* Testing

---

## Replica Set

```text
      Primary
      /     \
Secondary Secondary
```

Used for:

* Production systems
* High availability

---

## Sharded Cluster

```text
Client
   ↓
 mongos
   ↓
Shards
```

Used for:

* Large-scale applications
* Big data systems

---

# SQL vs MongoDB Architecture

| Relational DB    | MongoDB                         |
| ---------------- | ------------------------------- |
| Database         | Database                        |
| Table            | Collection                      |
| Row              | Document                        |
| Column           | Field                           |
| JOIN             | Embedded Documents / References |
| Vertical Scaling | Horizontal Scaling              |
| Fixed Schema     | Flexible Schema                 |

---

# Key Architectural Advantages

1. Flexible schema
2. JSON/BSON document model
3. High performance
4. Automatic failover through replication
5. Horizontal scalability through sharding
6. Rich indexing support
7. Distributed architecture
8. Suitable for cloud-native applications
