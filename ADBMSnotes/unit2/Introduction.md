# Introduction to NoSQL Databases

**NoSQL** stands for **“Not Only SQL.”** It refers to a group of database systems designed to store and process large volumes of  **structured, semi-structured, and unstructured data** .

Unlike traditional relational databases such as MySQL or Oracle, NoSQL databases generally do not require data to be stored in fixed rows and columns.

### 1. Why NoSQL?

Traditional relational databases work very well when:

* Data has a clearly defined structure.
* Relationships between data are important.
* Transactions require strong consistency.
* The database schema is relatively stable.

However, modern applications often deal with:

* Huge amounts of data
* Rapidly changing data
* JSON and other semi-structured formats
* Social-media data
* IoT sensor data
* Real-time applications
* Millions of users and transactions

NoSQL databases are designed to handle these situations efficiently.

---

## 2. SQL vs NoSQL

| Feature        | SQL Database              | NoSQL Database                     |
| -------------- | ------------------------- | ---------------------------------- |
| Data model     | Tables                    | Document, Key-Value, Column, Graph |
| Schema         | Usually fixed             | Flexible                           |
| Relationships  | Strong support            | Depends on database type           |
| Scaling        | Mainly vertical           | Mainly horizontal                  |
| Data           | Structured                | Structured + semi/unstructured     |
| Query language | SQL                       | Database-specific                  |
| Transactions   | Strong ACID support       | Varies by system                   |
| Examples       | MySQL, Oracle, PostgreSQL | MongoDB, Cassandra, Redis, Neo4j   |

---

## 3. Main Types of NoSQL Databases

There are  **four major types** :

### A. Document Database

Stores data as documents, commonly using JSON/BSON.

Example:

```json
{
  "student_id": 101,
  "name": "Rahul",
  "department": "CSE",
  "skills": ["Java", "MongoDB", "Python"]
}
```

**Example:** MongoDB

Useful for:

* Student management
* E-commerce
* Content management
* Web applications

---

### B. Key-Value Database

Stores data in the form:

```text
Key → Value
```

Example:

```text
"user101" → "Rahul"
"user102" → "Priya"
```

**Example:** Redis

Useful for:

* Caching
* Sessions
* Real-time applications

---

### C. Column-Family Database

Data is organized around columns rather than traditional rows.

**Examples:**

* Apache Cassandra
* HBase

Useful for:

* Big data
* IoT
* Large-scale distributed applications

---

### D. Graph Database

Stores information using:

* **Nodes** → entities
* **Relationships/Edges** → connections
* **Properties** → information about nodes/relationships

Example:

```text
(Rahul) ──FRIENDS_WITH──> (Amit)
   │
   └──STUDIES_AT──> (KIET)
```

**Example:** Neo4j

Useful for:

* Social networks
* Recommendation systems
* Fraud detection
* Network analysis

---

## 4. Important Characteristics of NoSQL

### Flexible Schema

Different documents can have different fields.

```json
{
  "name": "Rahul",
  "age": 20
}
```

Another document can contain additional information:

```json
{
  "name": "Priya",
  "age": 21,
  "skills": ["Python", "MongoDB"]
}
```

### Horizontal Scaling

Instead of continuously making one server more powerful, NoSQL databases can distribute data across multiple servers.

```text
             NoSQL Database
                  |
        ---------------------
        |         |         |
      Server 1  Server 2  Server 3
```

This is particularly useful for very large applications.

### High Availability

Many NoSQL systems replicate data across multiple servers so that the application can continue operating even if one server fails.

---

## 5. ACID vs BASE

Relational databases traditionally emphasize **ACID** properties:

* **A** – Atomicity
* **C** – Consistency
* **I** – Isolation
* **D** – Durability

Many distributed NoSQL systems historically emphasized the **BASE** approach:

* **B** – Basically Available
* **S** – Soft State
* **E** – Eventually Consistent

The important point is that **NoSQL does not mean “no consistency” or “no transactions.”** Modern NoSQL databases can provide varying levels of consistency and transactional support.

---

## 6. Real-World Example

Imagine an  **e-commerce website** .

A product may have:

```json
{
  "product": "Laptop",
  "price": 65000,
  "brand": "ABC",
  "ram": "16GB",
  "storage": "512GB SSD",
  "reviews": [
    {
      "user": "Rahul",
      "rating": 5
    }
  ]
}
```

With a relational database, this information might require several related tables.

With a document database such as MongoDB, much of this information can be stored together in a single document.

---

## 7. Advantages of NoSQL

✅ Flexible schema
✅ Handles large volumes of data
✅ Horizontal scalability
✅ High availability
✅ Suitable for distributed systems
✅ Good performance for specific workloads
✅ Works well with semi-structured and unstructured data

## 8. Limitations

❌ Different NoSQL databases use different query models
❌ Complex relationships may be harder in some NoSQL systems
❌ Data consistency guarantees vary
❌ Not every application benefits from NoSQL
❌ Requires careful data modeling

---
