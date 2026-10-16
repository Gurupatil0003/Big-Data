
## 1.1 What Is Avro?

**Definition:** Apache Avro is a schema-based data serialization system used to represent, store, and exchange structured data efficiently.

In simple words, Avro converts structured records into a compact binary representation and uses a schema to understand how the data should be decoded.

### Example: Student Record

```text
ID: 101
Name: Ravi
Marks: 85
```

Avro can serialize this record into binary data. The schema describes the fields and their data types.

---

## 1.2 Why Do We Need Avro?

In distributed systems such as Hadoop, data frequently moves between applications, machines, and processing jobs.

Avro provides the following advantages:

1. **Compact Storage:** Binary encoding can reduce the size compared with verbose text representations.
2. **Schema-Based Structure:** Fields and data types are explicitly defined.
3. **Language Interoperability:** Applications written in different supported programming languages can exchange data.
4. **Schema Evolution:** Records can evolve over time, subject to Avro's schema compatibility rules.
5. **Data Exchange:** Avro can transfer structured records between distributed systems.

---

## 1.3 How Does Avro Work?

Avro follows a serialization and deserialization process.

### Step 1: Define a Schema

Specify the record fields and their data types.

### Step 2: Create a Record

Example:

```text
ID: 101
Name: Ravi
Marks: 85
```

### Step 3: Serialize

Avro encodes the record into binary data according to the schema.

### Step 4: Deserialize

The receiving application uses the appropriate schema to decode the binary data and reconstruct the record.

### Working Flow

```text
Structured Data
      |
      v
  Avro Schema
      |
      v
   Serialize
      |
      v
  Binary Data
      |
      v
  Deserialize
      |
      v
Original Record
```

---

## 1.4 Example of an Avro Schema

Avro schemas are commonly written in JSON.

### Student Schema

```json
{
  "type": "record",
  "name": "Student",
  "fields": [
    {
      "name": "id",
      "type": "int"
    },
    {
      "name": "name",
      "type": "string"
    },
    {
      "name": "marks",
      "type": "int"
    }
  ]
}
```

### Explanation of the Schema

- **`type: record`** — Defines a structured record.
- **`name: Student`** — Gives the record a name.
- **`fields`** — Lists the fields in the record.
- **`id`** — Represents the student's ID.
- **`name`** — Represents the student's name.
- **`marks`** — Represents the student's marks.
- **`int`** — Specifies an integer data type.
- **`string`** — Specifies a string data type.

**Important:** The JSON shown above is the schema definition, not the serialized student record itself.

The schema describes the structure of the data, while serialization converts the actual record into binary form.

---

## 1.5 Where Is Avro Used?

Apache Avro is commonly used in the following scenarios:

1. **Hadoop Data Processing:** Storing and exchanging structured records in Hadoop-based applications.
2. **Distributed Applications:** Transferring data between applications running on different machines.
3. **Schema Evolution:** Managing changes to data structures over time.
4. **Data Pipelines:** Exchanging structured records between different processing systems.
5. **Multi-Language Applications:** Sharing data between applications written in different supported programming languages.

---

## 1.6 Important Exam Points

1. Apache Avro is a schema-based data serialization system.
2. Avro schemas are commonly defined using JSON.
3. Avro supports compact binary serialization.
4. Serialization converts structured records into an encoded representation.
5. Deserialization reconstructs records from serialized data.
6. Avro supports multiple programming languages.
7. Schema evolution allows data structures to change while maintaining compatibility when the applicable rules are satisfied.
8. Avro is useful for data exchange and Hadoop-based data processing.

---

## 1.7 Quick Revision

| Question | Answer |
|---|---|
| What is Avro? | A schema-based data serialization system. |
| Which format is commonly used to define Avro schemas? | JSON. |
| What is serialization? | Converting structured data into an encoded representation. |
| What is deserialization? | Reconstructing data from its serialized representation. |
| What is an Avro schema? | A definition of record fields and their data types. |
| What is schema evolution? | Changing a schema over time while managing compatibility. |
| What is the main advantage of Avro? | Compact serialization and schema-based data exchange. |
| Where is Avro used? | Hadoop, distributed applications, and data pipelines. |

---

## Conclusion

Apache Avro is an important data serialization technology used in distributed systems and Hadoop applications. It defines data structures using schemas and supports compact binary encoding, interoperability, and schema evolution.

**Remember:** Avro uses a schema to describe the structure of data and supports serialization and deserialization for efficient data exchange.
