# High-Level-design

System Design Quick Cheat Sheet - Visual + Code + Diagrams

---

# 1️⃣ API Layer

- **CRUD Basics:**
  - GET /resource → read
  - POST /resource → create
  - PUT /resource/{id} → update
  - DELETE /resource/{id} → delete
- **Request/Response:** JSON
- **Error codes:** 200, 201, 400, 404, 500
- **Auth:** JWT / API Key / OAuth
- **Tip:** Draw **request → controller → service → DB → response**

**Example:**
```http
POST /users
{
  "name": "Kavya",
  "email": "k@example.com"
}

Response:
201 Created
{
  "id": "u123",
  "name": "Kavya",
  "email": "k@example.com"
}
```

---

# 2️⃣ Database Storage Models

| DB           | Storage Type       | Structure / Example                           | When to Use / Memory Tip                                   |
|--------------|-----------------|-----------------------------------------------|------------------------------------------------------------|
| MongoDB      | Document DB       | JSON documents in collections                 | Flexible schema, embed or reference; think “objects”      |
| DynamoDB     | Key-Value / Wide Column | Item = key + attributes (JSON-like)        | Scalable, fast lookup by PK; good for massive tables      |
| Cassandra    | Wide Column       | Rows with flexible columns, partition key + clustering key | High write throughput, time-series, distributed           |
| Postgres     | Relational DB     | Table → rows → columns, strict schema         | ACID, joins, traditional relational data                 |

**Examples:**

- MongoDB:
```json
{
  "_id": "u123",
  "name": "Kavya",
  "orders": [{"orderId": "o1", "total": 25}]
}
```

- DynamoDB:
```json
{ "userId": "u123", "orderId": "o1", "total": 25 }
```

- Cassandra:
```
userId | orderId | total
u123   | o1      | 25
```

- Postgres:
```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name TEXT,
  email TEXT UNIQUE
);
```

---

# 3️⃣ Redis Data Structures

| Type        | Example                                   | Use Case / Memory Tip                        |
|-------------|-------------------------------------------|----------------------------------------------|
| String      | SET user:123 "Kavya"                    | Cache full object / session                 |
| Hash        | HSET user:123 name "Kavya"              | Store object fields individually            |
| List        | LPUSH queue "job1"                       | Simple queue                                 |
| Set         | SADD online_users 123                     | Unique elements, fast membership            |
| Sorted Set  | ZADD leaderboard 100 "user1"            | Leaderboard / top-k scoring                 |

---

# 4️⃣ Kafka vs SQS

| System | Type                 | Storage Model / Key Idea                     | Interview Tip                             |
|--------|--------------------|---------------------------------------------|-------------------------------------------|
| Kafka  | Distributed log      | Append-only logs, partitions, offsets       | Durable, replayable, for streaming events |
| SQS    | Queue                | FIFO or standard queue                       | Simple async decoupling, invisible until ack |

**Diagram:**
```
Client → API → Service → DB
                   ↓
                 Cache / Queue
```

---

# 5️⃣ Memory Tips for Interviews

- Think visually: Draw **client → API → DB → cache/queue**
- Associate DB with structure:
  - Mongo → JSON document
  - Dynamo → key-value item
  - Cassandra → wide table
  - Postgres → strict relational table
- Redis = fast in-memory store (string, hash, list, sorted set)
- Kafka = log stream; SQS = simple queue
- API CRUD pattern = resource + action + JSON

- Always clarify:
  - Read vs write path
  - Consistency vs availability tradeoff
  - Why you choose this DB / cache / queue
