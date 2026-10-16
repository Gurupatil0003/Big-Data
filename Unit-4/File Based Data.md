
# File-Based Data Structures in Hadoop

## 2.1 What Does This Topic Mean?

**Definition:** File-based data structures are ways of organizing and storing data in files so that applications can access, process, and retrieve the data efficiently.

In Hadoop, data is not always stored as plain text. Different file formats organize data into records, key-value pairs, or indexed structures to improve storage and retrieval.

### Example

Suppose a file contains millions of student records. Searching through every line may be inefficient. An appropriate file structure can organize the records for better access.

---

## 2.2 Important File-Based Data Structures

For this syllabus topic, focus particularly on **SequenceFile** and **MapFile**.

### A. SequenceFile

**Definition:** SequenceFile is a Hadoop binary file format that stores data as a sequence of key-value pairs.

### Example

```text
(101, Ravi)
(102, Priya)
(103, Arun)
```

Here:

- `101`, `102`, and `103` are keys.
- `Ravi`, `Priya`, and `Arun` are corresponding values.
- Each record contains a key-value pair.

### Uses

- Storing large collections of key-value records.
- Storing binary data in Hadoop.
- Supporting data exchange between Hadoop processing tasks.
- Storing intermediate results in suitable Hadoop workflows.

**Important:** A SequenceFile does not require its keys to be sorted.

### B. MapFile

**Definition:** MapFile is a specialized Hadoop file structure that stores sorted key-value pairs and maintains an index to help locate records efficiently.

### Example

```text
Key 101 → Ravi
Key 102 → Priya
Key 103 → Arun
```

The records are stored in sorted key order, and an index helps Hadoop locate the required key.

### Uses

- Searching for records using a key.
- Looking up student details using student ID.
- Accessing stored values more efficiently than scanning every record sequentially.

**Important:** A MapFile requires its keys to be sorted when it is constructed.

### C. Related Hadoop Data Structures

Hadoop also provides specialized structures such as `ArrayFile`, `SetFile`, and `BloomMapFile`.

| Data Structure | Description |
|---|---|
| `ArrayFile` | Stores values addressed by integer positions. |
| `SetFile` | Stores a set of keys. |
| `BloomMapFile` | Extends MapFile with a Bloom filter to help identify keys that are definitely absent. |

A Bloom filter may produce false positives, but it does not produce false negatives when used correctly.

---

## 2.3 SequenceFile vs MapFile

| Feature | SequenceFile | MapFile |
|---|---|---|
| Structure | Sequence of key-value pairs | Sorted key-value pairs with an index |
| Keys | Not necessarily sorted | Sorted when constructed |
| Index | No MapFile-style lookup index | Maintains an index |
| Main Purpose | Store records sequentially | Support efficient key-based lookups |
| Access | Sequential record processing | Key-based lookup and sequential access |
| Example | Store intermediate key-value records | Find a student record using student ID |

### Simple Difference

- **SequenceFile:** Used to store a sequence of key-value records.
- **MapFile:** Used to store sorted key-value records with an index for efficient lookup.

**Remember:** A MapFile is not simply any file containing a map. It is a specific Hadoop file-based data structure built around a SequenceFile and its index.

---

## 2.4 Other File Formats Used in Hadoop

Hadoop works with several file formats, depending on the application and processing requirements.

| File Format | Description | Common Use |
|---|---|---|
| Text File | Stores human-readable text records | CSV files, logs, and simple datasets |
| SequenceFile | Stores binary key-value pairs | Hadoop data processing |
| Avro | Stores schema-based serialized records | Data exchange and schema evolution |
| Parquet | Uses column-oriented storage | Analytical queries and column-based processing |
| ORC | Uses column-oriented storage with additional optimizations | Analytical workloads, especially in Hive |

### Row-Based vs Column-Based Storage

**Row-based storage:** Stores the fields of each record together.

Example:

```text
101, Ravi, 85
102, Priya, 90
103, Arun, 78
```

**Column-based storage:** Stores values for the same column together.

Example:

```text
ID:    101, 102, 103
Name:  Ravi, Priya, Arun
Marks: 85, 90, 78
```

Column-oriented formats such as Parquet and ORC can be efficient for analytical queries that read only selected columns.

---

## 2.5 Important Exam Points

1. File-based data structures organize data for efficient storage and access.
2. SequenceFile is a Hadoop binary file format that stores key-value pairs.
3. SequenceFile keys do not have to be sorted.
4. MapFile stores sorted key-value pairs and maintains an index.
5. MapFile is useful for key-based lookups.
6. ArrayFile stores values addressed by integer positions.
7. SetFile stores a set of keys.
8. BloomMapFile uses a Bloom filter to help identify keys that are definitely absent.
9. Avro uses schema-based serialization.
10. Parquet and ORC are column-oriented file formats.
11. The choice of file format depends on the data structure and processing requirements.

---

## 2.6 Quick Revision

| Question | Answer |
|---|---|
| What is a SequenceFile? | A Hadoop binary file format that stores key-value pairs. |
| What is a MapFile? | A Hadoop structure containing sorted key-value pairs and an index. |
| Must SequenceFile keys be sorted? | No. |
| Must MapFile keys be sorted? | Yes, when the MapFile is constructed. |
| What is the purpose of a MapFile index? | To help locate records by key efficiently. |
| What is an ArrayFile? | A file structure that stores values addressed by integer positions. |
| What is a SetFile? | A file structure used to store a set of keys. |
| What is a BloomMapFile? | A MapFile enhanced with a Bloom filter. |
| What is Avro? | A schema-based serialization format. |
| What are Parquet and ORC? | Column-oriented file formats used for analytical processing. |

---

## Conclusion

File-based data structures are important in Hadoop because they determine how data is organized, stored, and accessed.

SequenceFile is useful for storing key-value records, while MapFile supports key-based lookups through sorted records and an index. Other formats, including Avro, Parquet, and ORC, support different data storage and analytical requirements.

**Remember:** SequenceFile focuses on storing key-value records, whereas MapFile adds sorted keys and an index for efficient lookup.
