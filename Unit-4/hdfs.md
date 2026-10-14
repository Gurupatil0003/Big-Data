## Hdfc Architecture

https://medium.com/@gurupatil327/hdfs-hadoop-file-system-7535842a60b9?postPublishedType=initial

# Java Interface
# Java Interface in Hadoop (HDFS)

In Hadoop, Java Interface refers to using the Hadoop Java API to interact with HDFS (Hadoop Distributed File System).

It does not refer to the general Java `interface` keyword. The Hadoop Java API allows Java applications to perform file operations on HDFS programmatically.

## 1. Why Do We Need the Hadoop Java API?

Suppose you have a file named `students.txt` and want to store it in HDFS.

You can perform this operation in two ways:

**Using HDFS Commands:**

```bash
hdfs dfs -put students.txt /student/
```

Using Java: Write a Java program to upload a file to HDFS.

The Hadoop Java API allows applications to:

- Create directories
- Create and upload files
- Read files
- List files and directories
- Check whether files exist
- Delete files and directories

## 2. Important Hadoop Java Classes

| Class | Purpose |
|---|---|
| `Configuration` | Loads Hadoop configuration settings. |
| `FileSystem` | Provides methods to work with HDFS files and directories. |
| `Path` | Represents a file or directory path. |
| `FSDataOutputStream` | Writes data to an HDFS file. |
| `FSDataInputStream` | Reads data from an HDFS file. |

## 3. Java Program to Create a File in HDFS

This program creates a file named `students.txt` inside the `/student` directory in HDFS and writes student details into it.

```python
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.fs.FSDataOutputStream;

public class HDFSExample {

    public static void main(String[] args) throws Exception {

        Configuration conf = new Configuration();
        FileSystem fs = FileSystem.get(conf);

        Path directory = new Path("/student");

        if (!fs.exists(directory)) {
            fs.mkdirs(directory);
        }

        Path file = new Path("/student/students.txt");

        try (FSDataOutputStream out = fs.create(file, true)) {
            out.write("Guru 85\nRavi 90\n".getBytes());
        }

        fs.close();

        System.out.println("File created in HDFS successfully!");
    }
}
```

### How Does the Program Work?

1. `Configuration` loads Hadoop configuration settings.
2. `FileSystem.get(conf)` connects to the configured filesystem.
3. `Path` specifies the HDFS file or directory path.
4. `fs.mkdirs(directory)` creates the directory if it does not exist.
5. `fs.create(file, true)` creates the file and allows overwriting an existing file.
6. `out.write()` writes student details into the file.
7. `fs.close()` closes the filesystem connection.

## 4. How to Run and Verify the Program

### Step 1: Start HDFS

For a typical single-node Hadoop installation, run:

```bash
start-dfs.sh
```

### Step 2: Compile and Run the Java Program

Ensure that the Hadoop libraries are included in your Java classpath and that Hadoop is configured to connect to your HDFS instance.

You can use Maven with the appropriate `hadoop-common` and `hadoop-hdfs-client` dependencies for your Hadoop version.

### Step 3: Verify the File

Run the following command:

```bash
hdfs dfs -cat /student/students.txt

```

### Expected Output

```text
Guru 85
Ravi 90
```

**Note:** HDFS must be running, and the user executing the program must have permission to create the directory and file.

## 5. Common HDFS Operations Using the Java API

| Operation | Java API Method | Description |
|---|---|---|
| Create a directory | `fs.mkdirs(path)` | Creates a directory and missing parent directories. |
| Create a file | `fs.create(path)` | Creates a file in HDFS. |
| Read a file | `fs.open(path)` | Opens a file for reading. |
| Check file existence | `fs.exists(path)` | Checks whether a file or directory exists. |
| List files | `fs.listStatus(path)` | Lists the contents of a directory. |
| Delete a file | `fs.delete(path, false)` | Deletes a file. |
| Delete a directory | `fs.delete(path, true)` | Deletes a directory and its contents recursively. |


## 6. Java API vs Hadoop Commands

| Hadoop Commands | Hadoop Java API |
|---|---|
| Executed through a terminal. | Used inside Java applications. |
| Good for manual operations. | Good for automated operations. |
| Example: `hdfs dfs -ls /student` | Example: `fs.listStatus(path)` |
| Requires Hadoop command-line access. | Requires Hadoop Java libraries and filesystem configuration. |

