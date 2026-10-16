# Hadoop Compression — Complete Teaching Notes

## 1. What Is Compression?

**Definition:** Compression is the process of reducing the size of data by encoding it in a more compact representation, thereby reducing storage requirements and data transfer time.

### Simple Example

Suppose we have a file containing student records.

| Description | File Size |
|---|---:|
| Original file | 100 MB |
| Compressed file | 25 MB |
| Storage saved | 75 MB |

**Storage Saved:**

`Storage Saved = Original Size - Compressed Size`

`Storage Saved = 100 - 25 = 75 MB`

**Compression Ratio:**

$$
\text{Compression Ratio} = \frac{\text{Original Size}}{\text{Compressed Size}}
$$

$$
\text{Compression Ratio} = \frac{100}{25} = 4:1
$$

This means the compressed file is one-fourth of its original size, saving 75% of the storage space.

*Note: Actual compression depends on the dataset and compression algorithm.*

---

## 2. Why Do We Need Compression in Hadoop?

Hadoop is designed to process very large datasets, ranging from gigabytes to petabytes. Storing and transferring all this data without compression can consume substantial resources.

### Main Advantages

1. **Reduced Storage:** Reduces disk space required to store large datasets in HDFS.
2. **Reduced Network Traffic:** Transfers fewer bytes between cluster nodes.
3. **Improved Processing Performance:** Can reduce disk I/O and network transfer time.
4. **Reduced Storage Costs:** Less storage capacity may be required.
5. **Efficient Data Transfer:** Compressed data can be transferred more efficiently.

### Teaching Example

Suppose a Hadoop cluster processes 1 TB of text data. If compression reduces the dataset to 300 GB, fewer bytes need to be stored or transferred when using the compressed representation.

However, compression and decompression require CPU resources, so actual processing time depends on the codec, hardware, and workload.

---

## 3. How Does Compression Work in Hadoop?

### Working Process

1. **Original Data:** A large input file is available.
2. **Compression:** An algorithm encodes the data into a smaller representation.
3. **Storage or Transfer:** Compressed data is stored in HDFS or transferred across the cluster.
4. **Decompression:** Hadoop or the application decompresses the data when it needs to read it.

### What Is a Compression Codec?

A **compression codec** is a software component that implements a particular compression and decompression algorithm.

Examples:

- `GzipCodec` — Gzip compression
- `BZip2Codec` — BZip2 compression
- `SnappyCodec` — Snappy compression
- `Lz4Codec` — LZ4 compression
- `DefaultCodec` — Hadoop's default codec using the DEFLATE algorithm

---

## 4. Types of Compression

There are two main types of compression: lossless and lossy.

### A. Lossless Compression

**Definition:** Lossless compression reduces file size without losing any original information.

After decompression, the original data can be recovered exactly.

**Examples:**

- Gzip
- BZip2
- Snappy
- LZ4
- DEFLATE

**Applications:**

- Hadoop log files
- CSV files
- Transaction records
- Text datasets
- Database records

**Example:** If a student record contains a mark of `95`, lossless compression preserves that value exactly after decompression.

### B. Lossy Compression

**Definition:** Lossy compression reduces file size by permanently removing some information. The decompressed result may not be identical to the original.

**Examples:**

- JPEG images
- MP3 audio
- Some video compression formats

**Applications:**

- Images
- Audio
- Video

For typical Hadoop data-processing tasks, lossless compression is generally preferred because the original data values must be preserved.

---

## 5. Important Hadoop Compression Codecs

Hadoop supports several compression codecs, each offering different trade-offs between compression ratio, speed, CPU usage, and splittability.

| Codec | Main Characteristics | Common Use |
|---|---|---|
| Gzip | Good compression; generally not splittable as a single stream | Logs and text files |
| BZip2 | Good compression; supports splitting with compatible Hadoop input processing; relatively CPU-intensive | Large datasets where compression ratio matters |
| Snappy | Fast compression and decompression; generally not splittable as a single stream | Performance-sensitive data processing |
| LZ4 | Very fast compression and decompression | Workloads prioritizing speed |
| DEFLATE | General-purpose lossless compression algorithm | General data compression |

### What Is Splittability?

**Splittability** is the ability to divide a file into independently readable portions so that Hadoop can process different portions in parallel.

- **Splittable files:** Hadoop can divide input into multiple portions for parallel processing, provided the input format supports it.
- **Non-splittable compressed streams:** Hadoop generally needs to read the stream from its beginning, limiting parallel processing of different portions of that file.

**Example:** A large BZip2-compressed file can be split into suitable blocks for parallel processing, whereas a conventional Gzip-compressed file generally cannot be split in the same way.

*Note: Splittability depends on the compression format, file organization, and Hadoop input format.*

---

## 6. Gzip vs BZip2 vs Snappy

| Feature | Gzip | BZip2 | Snappy |
|---|---|---|---|
| Compression Ratio | Generally good | Often higher | Generally lower |
| Compression Speed | Moderate | Slower | Fast |
| Decompression Speed | Moderate | Relatively slower | Fast |
| CPU Usage | Moderate | Relatively high | Generally lower |
| Splittability | Generally non-splittable as a stream | Supports splitting | Generally non-splittable as a stream |
| Best Suited For | General-purpose compression | Higher compression ratios | Fast data processing |

These are general tendencies. Actual results depend on the dataset, codec implementation, hardware, and configuration.

### Which Codec Should We Choose?

- **Gzip:** Choose when general-purpose compression and a good compression ratio are important.
- **BZip2:** Choose when a higher compression ratio and splittable input processing are useful, and additional CPU time is acceptable.
- **Snappy:** Choose when fast compression and decompression are more important than achieving the smallest possible file.
- **LZ4:** Choose when high compression and decompression speed is a priority.

---

## 7. Where Is Compression Used in Hadoop?

Compression can be applied at different stages of Hadoop data processing.

### A. Input Compression

Input files are compressed before Hadoop reads them.

**Benefits:**

- Reduces storage requirements.
- Can reduce disk I/O.
- Can reduce the amount of data read from storage.

### B. Intermediate Compression

Intermediate data produced during MapReduce processing can be compressed.

**Benefits:**

- Reduces network traffic during the shuffle phase.
- Can improve performance when network transfer is a bottleneck.

### C. Output Compression

The output produced by a Hadoop job is compressed before being stored.

**Benefits:**

- Reduces HDFS storage requirements.
- Makes output files smaller.
- Can improve subsequent data transfers.

### D. SequenceFile Compression

Hadoop SequenceFiles can store key-value pairs using compression.

Common compression types include:

- **NONE:** No compression.
- **RECORD:** Compresses individual records.
- **BLOCK:** Compresses groups of records together.

Block compression can provide better compression efficiency because it processes multiple records as a group.

---

## 8. Important Exam Points

1. Compression reduces the size of data.
2. Hadoop compression helps reduce storage requirements and data transfer overhead.
3. A compression codec implements a compression algorithm.
4. Lossless compression preserves all original information.
5. Lossy compression permanently removes some information.
6. Gzip, BZip2, Snappy, and LZ4 are examples of lossless compression codecs.
7. BZip2 supports splitting in compatible Hadoop input processing.
8. Gzip and Snappy streams are generally not splittable as single compressed streams.
9. Intermediate compression can reduce network traffic during MapReduce shuffle.
10. Output compression reduces the storage space required for job results.
11. Compression may improve performance, but compression and decompression also consume CPU resources.
12. The best codec depends on compression ratio, speed, CPU usage, and splittability.

---

## 9. Quick Revision

| Question | Answer |
|---|---|
| What is compression? | Reducing data size using an encoding algorithm. |
| Why is compression used in Hadoop? | To reduce storage, disk I/O, and data transfer overhead. |
| What is a codec? | A component that implements a compression algorithm. |
| What is lossless compression? | Compression that preserves all original information. |
| What is lossy compression? | Compression that permanently removes some information. |
| Which codec supports splittable compressed input? | BZip2, with compatible Hadoop input processing. |
| Which codecs are commonly chosen for speed? | Snappy or LZ4, depending on the workload. |
| What is intermediate compression? | Compression of intermediate data during processing. |
| What is output compression? | Compression of the final job output. |
| What is the main trade-off? | Compression ratio and reduced I/O versus CPU overhead. |

---

## Conclusion

Compression is an important Hadoop feature that helps manage large datasets efficiently. By reducing data size, it can save storage space, decrease network traffic, and improve processing performance.

Choosing the appropriate compression codec depends on the dataset, required compression ratio, processing speed, CPU availability, and whether parallel processing of compressed input is needed.
