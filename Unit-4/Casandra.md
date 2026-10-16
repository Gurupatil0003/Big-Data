
# 3. Apache Cassandra

## 3.1 What Is Cassandra?

**Definition:** Apache Cassandra is a distributed NoSQL database designed to store and manage large volumes of data across multiple machines while providing high availability and scalability.

In simple words, Cassandra is a database that distributes data across multiple servers instead of relying on a single machine.

It is especially useful for applications that need to handle large volumes of writes and remain available across multiple nodes.

---

## 3.2 Why Do We Need Cassandra?

Imagine an online shopping platform generating millions of events:

- Customer logins
- Product views
- Orders
- Payment events
- Application activity

A single database server may become a bottleneck as data volume and traffic increase.

Cassandra distributes data across multiple nodes, allowing the system to scale horizontally.

### Main Benefits

1. **Scalability:** Add nodes to increase storage capacity and processing capability.
2. **High Availability:** Replicate data across multiple nodes to improve availability.
3. **Distributed Storage:** Spread data across a cluster of machines.
4. **High Write Throughput:** Designed to handle write-intensive workloads.
5. **Flexible Data Model:** Uses a wide-column NoSQL model rather than a traditional relational database design.

---

## 3.3 Cassandra Architecture

Cassandra uses a **peer-to-peer architecture**, in which nodes cooperate to store and manage data without depending on a single central master node.

### Architecture Diagram

```text
             Application
                  |
                  v
       Read / Write Requests
                  |
                  v
        +-------------------+
        | Cassandra Cluster |
        +-------------------+
           /      |      \
          v       v       v
      +------+ +------+ +------+
      |Node 1| |Node 2| |Node 3|
      +------+ +------+ +------+
          \       |       /
           \      |      /
            Data Distribution
            and Replication
```

Data is distributed across nodes and may be replicated according to the configured replication strategy.

### Important Terms

- **Node:** An individual machine running Cassandra.
- **Cluster:** A group of Cassandra nodes working together.
- **Replication:** Maintaining copies of data on multiple nodes.
- **Partition:** The logical portion of data assigned according to its partition key.
- **CQL:** Cassandra Query Language, which has syntax similar to SQL.
- **Peer-to-Peer Architecture:** An architecture in which nodes cooperate without a single central master handling every operation.

---

## 3.4 How Is Data Stored in Cassandra?

Cassandra uses a **wide-column data model**. Data is organized into tables, and the primary key determines partitioning and how records are identified.

### Example: Create a Keyspace

A keyspace is a top-level namespace that contains tables and defines replication settings.

```sql
CREATE KEYSPACE college
WITH replication = {
    'class': 'SimpleStrategy',
    'replication_factor': 3
};
```

Here:

- `college` is the keyspace name.
- `SimpleStrategy` is a replication strategy generally intended for a single data center or simple setups.
- `replication_factor: 3` requests three replicas of each partition.

**Note:** The cluster must have enough suitable nodes to place the requested replicas.

### Create a Table

```sql
CREATE TABLE college.students (
    student_id int PRIMARY KEY,
    name text,
    marks int
);
```

Here:

- `student_id` identifies each row.
- `name` stores the student's name.
- `marks` stores the student's marks.
- `PRIMARY KEY` defines the key used to identify and organize the data.

### Insert a Record

```sql
INSERT INTO college.students
(student_id, name, marks)
VALUES (101, 'Ravi', 85);
```

### Retrieve a Record

```sql
SELECT * FROM college.students
WHERE student_id = 101;
```

### Expected Result

| student_id | name | marks |
|---:|---|---:|
| 101 | Ravi | 85 |

**Important:** Cassandra's primary key includes a partition key and may also include clustering columns. In this example, `student_id` is the partition key because it is the only primary-key column.

---

## 3.5 Cassandra vs Hadoop

| Feature | Hadoop | Cassandra |
|---|---|---|
| Main Purpose | Distributed storage and large-scale data processing | Distributed database and data access |
| Storage | HDFS commonly stores large files | Stores partitioned database records |
| Processing | MapReduce and other ecosystem tools | Database queries and read/write operations |
| Typical Workload | Large-scale batch analytics | Continuous reads and writes |
| Scaling | Add machines to the cluster | Add nodes to the database cluster |
| Data Access | Often processes large datasets in batches | Supports low-latency access patterns when appropriately designed |

### Key Difference

- **Hadoop:** Commonly used to store and process large datasets for batch analytics.
- **Cassandra:** Designed to store distributed records and support ongoing database operations.

**Remember:** Hadoop and Cassandra are not direct replacements for each other. They solve different problems and can be used together in a data architecture.

---

# 4. Hadoop Integration

## 4.1 What Is Hadoop Integration?

**Definition:** Hadoop integration is the process of connecting Hadoop with external databases, storage systems, and processing tools so that data can be exchanged, stored, and analyzed.

Hadoop does not have to work alone. It can be part of a larger data architecture.

For example, a company might store real-time application events in Cassandra and use Hadoop tools to analyze historical data.

---

## 4.2 How Does Hadoop Integrate with Cassandra?

Hadoop and Cassandra can exchange data through compatible connectors, APIs, import/export jobs, or custom data pipelines.

### Architecture Diagram

```text
       Applications
             |
             v
   Customer Activity and Orders
             |
             v
       +------------+
       | Cassandra  |
       +------------+
             |
             v
   Connector / Data Pipeline
             |
             v
       +------------+
       |   Hadoop   |
       +------------+
             |
       +-----+-----+
       |           |
       v           v
     HDFS       MapReduce
       |           |
       +-----+-----+
             |
             v
       Analytics and Reports
             |
             v
 Historical Trends and Insights
```

The arrows represent possible data flows, not an automatic built-in connection. A compatible connector or application must be configured to move data between Cassandra and Hadoop.

### Explanation

1. **Applications:** Generate customer activity, orders, and events.
2. **Cassandra:** Stores distributed operational data.
3. **Data Pipeline:** Transfers or exposes the required data to Hadoop.
4. **HDFS:** Stores large files and datasets in a distributed manner.
5. **MapReduce:** Processes large datasets in parallel.
6. **Analytics:** Produces reports and insights from the processed data.

---

## 4.3 Practical Example: E-Commerce Analytics

Consider an e-commerce company that receives millions of order records.

### Working Process

1. The application generates order records.
2. Cassandra stores operational data for applications to access.
3. A data pipeline exports or exposes the required records to Hadoop.
4. Hadoop processes the data using a suitable batch-processing framework.
5. The results identify best-selling products, customer trends, and sales patterns.
6. Reports help the business make decisions.

### Example Output

| Product | Units Sold |
|---|---:|
| Laptop | 500 |
| Mobile Phone | 850 |
| Headphones | 620 |

The company can analyze these results to understand product performance and customer purchasing patterns.

This is one example of combining a distributed database with a distributed data-processing system.

---

## 4.4 Other Hadoop Integration Technologies

Depending on the Hadoop version and environment, Hadoop can integrate with several technologies.

| Technology | Purpose |
|---|---|
| Apache Hive | SQL-like querying and analysis of large datasets |
| Apache HBase | Distributed NoSQL database integrated with the Hadoop ecosystem |
| Apache Sqoop | Historically used for bulk data transfer between relational databases and Hadoop; now a legacy technology in many environments |
| Apache Flume | Collecting and moving log/event data into Hadoop storage |
| Apache Spark | Distributed data processing and analytics |
| Apache Cassandra | Distributed NoSQL database that can exchange data with Hadoop through compatible integration tools |

### Important Notes

- **Hive** provides a SQL-like interface for querying large datasets.
- **HBase** is a distributed database commonly associated with the Hadoop ecosystem.
- **Sqoop** was used to import and export bulk data between relational databases and Hadoop.
- **Flume** was designed to collect and transfer large volumes of log and event data.
- **Spark** supports distributed processing, analytics, and other workloads.
- **Cassandra** provides distributed database capabilities and can exchange data with Hadoop through compatible integration methods.

These technologies have different roles. Integration may involve connectors, import/export jobs, APIs, or data pipelines, depending on the technology and its version.

---

## 4.5 Important Exam Points

1. Apache Cassandra is a distributed NoSQL database.
2. Cassandra is designed for scalability and high availability.
3. Cassandra uses a peer-to-peer architecture.
4. A node is an individual machine in a Cassandra cluster.
5. Replication maintains copies of data across nodes.
6. CQL stands for Cassandra Query Language.
7. A keyspace is a namespace that contains tables and replication settings.
8. The primary key determines partitioning and row identification.
9. Hadoop is commonly used for distributed storage and large-scale data processing.
10. Cassandra is designed for distributed database workloads.
11. Hadoop and Cassandra can be integrated using compatible connectors and data pipelines.
12. Hive, HBase, Sqoop, Flume, and Spark provide different capabilities within data-processing architectures.

---

## 4.6 Quick Revision

| Question | Answer |
|---|---|
| What is Cassandra? | A distributed NoSQL database. |
| Why is Cassandra used? | To store and access large volumes of distributed data. |
| What is a node? | An individual machine running Cassandra. |
| What is a cluster? | A group of cooperating Cassandra nodes. |
| What is replication? | Maintaining copies of data across multiple nodes. |
| What is CQL? | Cassandra Query Language. |
| What is a keyspace? | A namespace containing tables and replication settings. |
| What is Hadoop integration? | Connecting Hadoop with other systems to exchange and process data. |
| What is HDFS? | Hadoop Distributed File System. |
| What is MapReduce? | A Hadoop programming model for distributed data processing. |
| What is Apache Hive? | A technology that provides SQL-like querying for large datasets. |
| What is Apache HBase? | A distributed NoSQL database associated with the Hadoop ecosystem. |

---

## Conclusion

Apache Cassandra is a distributed NoSQL database designed to provide scalable data storage and efficient database operations across multiple machines.

Hadoop focuses on distributed storage and large-scale data processing, while Cassandra focuses on distributed database workloads. Integrating the two systems allows organizations to combine operational data storage with large-scale analytics.

**Remember:** Cassandra stores and manages distributed records, while Hadoop processes large datasets. Connectors and data pipelines enable them to work together.
