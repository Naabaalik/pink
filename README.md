8 V’s of Big Data
Big Data Is An Evolving Term That Describes Any Voluminous Amount Of Structured, Semi-structured And Unstructured Data That Has The Potential To Be Mined For Information.
The 8 V’s of big data are:
· Volume: refers to the massive amount of data generated every day by individuals, organizations, and devices.
· Velocity: refers to the speed at which data is generated and processed.
· Variety: refers to the different types of data, including structured, semi-structured, and unstructured data.
· Variability: refers to the inconsistencies in data quality and format.
· Veracity: refers to the accuracy and trustworthiness of the data.
· Value: refers to the potential business value that can be derived from the data.
· Visualization: refers to the process of representing data in graphical or pictorial form for easy understanding.
· Complexity: refers to the difficulty in processing and analyzing big data due to its sheer size and variety.
HDFS Architechture
HDFS (Hadoop Distributed File System) is the underlying file system of Hadoop. It is a distributed file system designed to store large data sets across multiple commodity servers, providing high throughput access to this data.
· Individual files are broken into blocks of fixed size (typically 256 MB blocks)
· Stored across cluster of nodes (not necessarily on same machine)
· These files can be more than the size of individual machine’s hard drive
· So access to a file requires cooperation of several nodes
Design Challenges
· System expects large files for processing ; small number of very large files would be stored
· Several machines involved in storing a file, loss of machine should be handled
· Block size to be important consideration, this is to keep smaller metadata at the node for each file
The HDFS architecture consists of:
· NameNode: The master node that manages the file system namespace and regulates access to files by clients. Name Node is controller and manager of HDFS. It knows the status and the metadata of all the files in HDFS. HDFS cluster can be accessed concurrently by multiple clients, even then this metadata information is never desynchronized; hence, all this information is handled by a single machine. Since metadata is typically small, all this info is stored in main memory of Name Node, allowing fast access to metadata
· DataNode: The slave nodes that store the actual data blocks. It knows only about the data stored on it and will read data and send to client when retrieval requested. It will receive data and store locally when storage is requested.
· Secondary NameNode: An auxiliary node that helps the NameNode manage its file system metadata, including the namespace and file-to-block mapping. It is not backup of name node nor data nodes connect to this; it is just a helper of name node. It only performs periodic checkpoints. It communicates with name node and to take snapshots of HDFS metadata. These snapshots help minimize downtime and loss of data
· Client: The component that interacts with HDFS, submits read/write requests, and receives responses.
In HDFS, data is split into blocks and distributed across multiple DataNodes for storage. The NameNode maintains metadata about the locations of the blocks, enabling it to serve clients’ read/write requests and manage data replication across the cluster.
HDFS Read
Here are the steps involved in reading a file from HDFS:
1. Client sends a read request to the NameNode for the desired file.
2. NameNode responds with the block location information for the requested file.
3. Client establishes a connection with the DataNode that holds the first block of the file.
4. Client retrieves the block of data from the DataNode.
5. If the file is stored in multiple blocks, the client repeats steps 3 and 4 for each remaining block.
6. The client reassembles the retrieved blocks into the original file.
7. The client receives the requested file from HDFS.
In HDFS, data is stored in a redundant manner across multiple DataNodes to ensure reliability and availability. The NameNode is responsible for ensuring that data is available even in the case of a node failure.
HDFS Write
Here are the steps involved in writing a file to HDFS:
1. Client creates a file and opens it for writing.
2. Client splits the file into blocks and stores each block in a local buffer.
3. Client sends a write request to the NameNode to create a new file in HDFS.
4. NameNode returns a list of DataNodes where the blocks of the file should be stored.
5. Client writes the first block of the file to the first DataNode in the list.
6. Client repeats step 5 for each remaining block, writing the blocks to their corresponding DataNodes.
7. The DataNodes acknowledge the receipt of each block and store it locally.
8. The NameNode updates its metadata to reflect the addition of the new file and the locations of its blocks.
9. Client closes the file, completing the write process.
In HDFS, data is stored in a redundant manner across multiple DataNodes to ensure reliability and availability. The NameNode is responsible for ensuring that data is available even in the case of a node failure by replicating blocks across the cluster.
What is MAP REDUCE with Example
MapReduce is a programming model and an associated implementation for processing and generating large data sets with a parallel, distributed algorithm on a cluster.
In MapReduce, the data is first divided into smaller chunks, and then a map function is applied to each chunk in parallel, transforming the data into intermediate key-value pairs. These intermediate pairs are then sorted and grouped by key, and a reduce function is applied to each group, aggregating the values into a final output.
Here’s a simple example to help illustrate the process:
Suppose we have a large dataset of text documents and we want to count the number of occurrences of each word in the documents.
· The map function takes as input a single document and outputs a series of key-value pairs, where each pair consists of a word from the document and the value 1.
· The reduce function takes as input a word and a list of its associated values (1s), and outputs a single key-value pair, where the key is the word and the value is the total number of occurrences of the word in the input data.
· The MapReduce framework takes care of dividing the data into chunks, distributing the map and reduce operations to multiple machines, and combining the intermediate results into a final output.
This example demonstrates how MapReduce can be used to perform complex data processing and aggregation tasks in a parallel and scalable manner. The model can be applied to a wide range of problems, from counting the frequency of words in text to calculating statistics on large datasets.
MAP REDUCE STEPS
MapReduce is a programming model for processing and generating large data sets with a parallel, distributed algorithm on a cluster. The following are the steps involved in a MapReduce job:
1. Input: The input data is divided/ splited into smaller chunks and distributed across the cluster.
2. Job needs to be scheduled to carry out required process
3. Schedule tasks on nodes where data is already present
4. Map: A map function is applied to each chunk of data in parallel, producing a set of intermediate key-value pairs.
5. Shuffle and Sort: The intermediate key-value pairs are sorted and grouped by key, producing a set of key-value lists.
6. Reduce: A reduce function is applied to each group of intermediate key-value pairs, aggregating the values into a final output.
7. Output: The final output is collected and stored in the desired format.
The MapReduce framework takes care of managing the distribution of data and tasks across the cluster, as well as handling failures and network communication. This allows developers to focus on writing the map and reduce functions, while the framework provides the underlying infrastructure for parallel and scalable processing.
What is MR-V1 and its PROCESS
MRv1, short for MapReduce Version 1, is an earlier implementation of the MapReduce programming model for processing and generating large data sets with a parallel, distributed algorithm on a cluster. It was the original implementation of MapReduce and was part of the Hadoop ecosystem.
In MRv1, the processing was managed by a single component called the JobTracker, which was responsible for scheduling and coordinating MapReduce tasks on a cluster of worker nodes. The JobTracker would also monitor the health of the tasks and manage the distribution of data to ensure optimal processing.
MRv1 had a number of limitations, including a single point of failure (the JobTracker), poor scalability, and limited resource management capabilities.
MRv1 Process:
· A Client invokes a Map Reduce, from a Calling Node (maybe a Data Node or an Extra Node in the cluster)
· An instance of Job Tracker is created in the memory of the Calling Node
· The Job Tracker queries the Name Node and finds the Data Nodes (location of the data to be used)
· Job Tracker then launches Task Trackers in the memory of all the Data Nodes as above to run the jobs
· Job Tracker gives the code to Task Tracker to run as a Task
· Task Tracker is responsible for creating the tasks & running the tasks
· In effect the Mapper of the Job is found here
· Once the Task is completed, the result from the Tasks is sent back to the Job Tracker
· Job Tracker also keeps a track of progress by each Task Tracker
· The Job Tracker also receives the results from each Task Tracker and aggregates the results
· In effect the Reducer of the Job is found here
MR-V2 and its PROCESS
MRv2, short for MapReduce Version 2, is a more recent implementation of the MapReduce programming model for processing and generating large data sets with a parallel, distributed algorithm on a cluster.
MRv2, also known as YARN (Yet Another Resource Negotiator), introduced a more flexible and scalable architecture for processing large data sets. Unlike MRv1, which had a single component (the JobTracker) managing the processing, MRv2 separates the processing management into two components: the Resource Manager and the Application Master.
The Resource Manager is responsible for allocating resources (such as CPU and memory) to running applications and managing the allocation of containers (units of processing resources). The Application Master is responsible for negotiating resources from the Resource Manager, tracking the progress of the tasks, and responding to failures.
With MRv2, multiple applications can run on the same cluster simultaneously, each with its own Application Master. This provides improved scalability and more advanced resource management capabilities compared to MRv1.
MRv2 Process:
· A Client invokes a Map Reduce, from a Calling Node (maybe a Data Node or an Extra Node in the cluster)
· An instance of Resource Manager is created in the memory of the Calling Node
· The Resource Manager then launches containers with appropriate resources (memory) with App Node Manager in memory of the Calling Node
· Along with this Application Master is invoked. Application Master is “pause” mode till all containers
· With Task Node Manager (as below) are created
· The Resource Manager queries the Name Node and finds the Data Nodes (location of the data used)
· The Resource Manager then launches containers with appropriate resources (memory) with Task Node Manager in all the Data Nodes as above to run the jobs
· Application Master gives the code to Task Node Manager to run as a Task
· Task Node Manager is responsible for creating & running tasks. In effect the Mapper of the Job is here
· Once the Task is completed, the result from the Tasks is sent back to the Application Master
· Application Master also keeps a track of progress by each Task Node Manager
· The Application Master also receives the results from each Task Node Manager and aggregates the results
· In effect the Reducer of the Job is found here
· Thus, from previous version, Job Tracker has been replaced by Resource Manager & Application Master
· From previous version, Task Tracker has been replaced by Task Node Managers
MAP REDUCE FAILURE RECOVERY
MRv1
· The processing was managed by a single component called the JobTracker. The JobTracker was responsible for scheduling and coordinating MapReduce tasks on a cluster of worker nodes.
· In case of a failure of a task or a node, the JobTracker would detect the failure and reschedule the task on another node.
· If the JobTracker failed, all running MapReduce jobs would be lost, and the cluster would become unavailable until the JobTracker was manually restarted.
MRv2
· If a task or node fails, the Application Master can detect the failure and reschedule the task on another node.
· The Resource Manager can also monitor the health of the cluster and take corrective actions in case of failures.
· Additionally, MRv2 allows for multiple applications to run on the same cluster simultaneously, so if one application experiences a failure, the other applications can continue to run without interruption.
· The data processed is also stored in a highly-reliable and scalable storage system, such as HDFS, which provides built-in mechanisms for data replication and recovery.
· Ensure that the data remains safe and accessible even in case of node failures.
· Task Failure: new task is started by Task Node Manager
· Task Node Manager Failure: new container with Task Node Manager is created by Resource Manager this Task Node Manager is given the code and started by Application Master
· Application Master Failure: New Application Master is started by App Node Manager
· App Node Manager Failure: new container with App Node Manager is created by Resource Manager this App Node Manager invokes the Application Master
· Resource Manager Failure: new resource manager with saved state is started
Why YARN is considered more fault tolerant and safer than MRv1:
YARN (Yet Another Resource Negotiator) is considered to be more fault tolerant and safer than MRv1 (MapReduce version 1) for several reasons:
1. Resource Management: YARN is a more advanced resource manager than MRv1, allowing for dynamic allocation of resources to applications based on their needs. This helps prevent overloading of nodes, which can lead to failures.
2. Fault Tolerance: YARN supports the ability to recover from node failures by automatically re-allocating work to other nodes. This increases the overall reliability of the system and helps prevent data loss.
3. Scalability: YARN provides the ability to scale out the cluster by adding more nodes, which helps prevent a single node from becoming a bottleneck. This leads to better performance and stability.
4. Improved Security: YARN supports fine-grained access control and provides better isolation between different applications running on the same cluster. This helps prevent unauthorized access to sensitive data.
In MRv1:
If the JobTracker failed, all running MapReduce jobs would be lost, and the cluster would become unavailable until the JobTracker was manually restarted.
But in MRv2:
1. If a task or node fails, the Application Master can detect the failure and reschedule the task on another node.
2. The Resource Manager can also monitor the health of the cluster and take corrective actions in case of failures.
3. Additionally, MRv2 allows for multiple applications to run on the same cluster simultaneously, so if one application experiences a failure, the other applications can continue to run without interruption.
4. The data processed is also stored in a highly-reliable and scalable storage system, such as HDFS, which provides built-in mechanisms for data replication and recovery.
5. Also, ensure that the data remains safe and accessible even in case of node failures.
In summary, YARN provides improved resource management, fault tolerance, scalability, and security compared to MRv1, making it a safer and more reliable platform for large-scale data processing.
Apache Hive Architecture
Apache Hive is a data warehousing tool that provides a SQL-like interface for querying and analyzing large datasets stored in Hadoop. It was developed by Facebook and later open-sourced under the Apache License. Apache Hive architecture comprises different components that work together to provide efficient querying and processing of large-scale data.
▪ Data warehouse system for Hadoop
▪ Run SQL-like queries that get compiled and run as Map Reduce jobs
▪ Displays the result back to the user
▪ Data in Hadoop even though generally unstructured has some vague structure associated with it
Features:
 ▪ Data Definition Language 
▪ Data Manipulation Language 
▪ SQL like Hive Queries 
▪ Pluggable input-output format 
▪ Extendible 
•	User Defined Functions — UDFs 
•	User Defined Aggregate Functions — UDAF 
•	User Defined Table Functions — UDTF
Case Sensitivity: 
▪ Most aspects of the Hive language are case-insensitive 
▪ Keywords & functions are definitely case-insensitive 
▪ Identifiers 
 • Col-Names — case-insensitive 
•	Table-Names / DB-Names — case-sensitive 
▪ Capitalizing keywords is a convention but not required 
▪ Note — string comparisons are case-sensitive (as always)
Hive — Partitions 
▪ Partitioning data is often used for distributing load horizontally 
▪ This is done for performance benefit and helps in organizing data in a logical fashion 
▪ Example like if we are dealing with large employee table and often run queries with WHERE clauses that restrict the results to a particular country or department 
▪ Hive table can be PARTITIONED BY (country STRING, department STRING) 
▪ Partitioning tables changes how Hive structures the data storage 
▪ Hive will now create subdirectories reflecting the partitioning structure like …/employees/country=ABC/DEPT=XYZ. 
▪ If query limits for employee from country ABC, Hive will only scan the contents of one directory ABC 
▪ This dramatically improves query performance, but only if the partitioning scheme reflects filtering 
▪ Partitioning feature is very useful in Hive, however, a design that creates too many partitions may optimize some queries, but be detrimental for other important queries. 
▪ Other drawback is having too many partitions is the large number of Hadoop files and directories that are created unnecessarily and consequent overhead on Name Node.
Hive — Buckets 
▪ Bucketing is another technique for decomposing data sets into more manageable parts 
▪ For example, suppose a table using the employee_id as the partition; result many small partitions 
▪ Instead, if we bucket the employee table and use employee_id as the bucketing column the value of this column will be hashed by a user-defined number into buckets 
▪ Records with the same employee_id will always be stored in the same bucket 
▪ Assuming the number of employee_id is much greater than the number of buckets, each bucket will have many employee_id 
▪ While creating table you can specify like CLUSTERED BY (employee_id) INTO XX BUCKETS ; where XX is the number of buckets 
▪ Bucketing has several advantages 
•	The number of buckets is fixed so it does not fluctuate with data 
•	If two tables are bucketed by employee_id, Hive can create a logically correct sampling 
•	Bucketing also aids in doing efficient map-side joins etc
The key components of the Apache Hive architecture are:
▪ Metastore stores the catalog and metadata about tables, columns, partitions, etc
▪ Driver manages the lifecycle of a HiveQL statement as it moves through Hive
▪ Query Compiler compiles HiveQL into Map-Reduce tasks
▪ Execution Engine executes the tasks produced by the compiler in proper dependency order
▪ System provides an interface to view the results in the Hive Client or over Web UI
1. Metastore: The Metastore is a central repository that stores metadata about the Hive tables, partitions, and their properties. It is essentially a database that stores schema information such as column names, data types, and storage location of tables. The Metastore can use different types of databases such as MySQL, Postgres, or Oracle to store metadata.
2. Driver: The Driver is the main component of the Hive architecture responsible for interacting with the user and executing Hive queries. It is a Java program that runs on the client-side and takes in HiveQL queries as input. The Driver translates the HiveQL queries into a series of MapReduce or Tez jobs that can be executed on the Hadoop cluster.
3. Query Compiler: The Query Compiler is responsible for parsing and optimizing the HiveQL queries submitted by the user. It is implemented as a set of Java classes that work in conjunction with the Driver. The Query Compiler first parses the queries and checks for syntax errors. It then optimizes the query by generating an execution plan that can be executed on the Hadoop cluster.
4. Execution Engine: The Execution Engine is responsible for executing the query plan generated by the Query Compiler. Hive supports multiple Execution Engines such as MapReduce, Tez, and Spark. The choice of Execution Engine depends on the configuration and capabilities of the Hadoop cluster. The Execution Engine reads the data from the Hadoop Distributed File System (HDFS) and processes it according to the query plan.
5. System: The System component comprises the Hadoop ecosystem components such as HDFS, MapReduce, and YARN. Hive is built on top of these components and leverages their capabilities to provide distributed data processing and storage.
In summary, the Metastore, Driver, Query Compiler, Execution Engine, and System components form the core of the Apache Hive architecture. The Metastore stores the metadata about the Hive tables, while the Driver interacts with the user and submits queries to the Query Compiler. The Query Compiler parses and optimizes the queries, and the Execution Engine executes the query plan. The System component comprises the Hadoop ecosystem components that provide distributed data processing and storage.
Apache spark, Difference and similarities between Hadoop and Spark
Apache Spark is a big data processing framework that provides a fast and general-purpose platform for large-scale data processing. It was developed by the Apache Software Foundation and is open-source, under the Apache License.
▪ Spark is used for data analytics in cluster computing framework
▪ Spark fits into the Hadoop open-source community, building on top of the Hadoop Distributed File System (HDFS)
▪ Spark provides an easier to use alternative to Hadoop Map-Reduce and offers performance up to 10 times faster than previous generation systems like Hadoop Map-Reduce for certain applications
▪ Spark is a framework for writing fast, distributed programs
▪ Spark solves similar problems as Hadoop Map-Reduce does but with a fast in-memory approach.
▪ Spark can be used interactively to quickly process and query big data sets
▪ Spark has inbuilt tools for
• interactive query analysis (Shark)
• large-scale graph processing and analysis (Bagel)
• real-time analysis (Spark Streaming)
Apache Spark — Architecture:
▪ Spark basically uses an in-memory MapReduce approach
▪ Spark revolves around the concept of a Resilient Distributed Dataset (RDD).
▪ RRD is a fault-tolerant collection of elements that can be operated on in parallel.
▪ There are two ways to create RDDs:
• parallelizing an existing collection in your driver program, Hadoop
• referencing a dataset in a shared filesystem like HDFS or a DBMS like HBase
Driver Program: Runs the main() function of the program and creates the SparkContext object.
SparkContext: Used to connect to the spark clusters. 
The Spark Driver is responsible for converting the user-written code into jobs that are executed on the clusters.
SparkContext performs the following:
1.	Acquire executor on nodes in the cluster
2.	Send application code to the executors
3.	Send tasks to the executors to run

Cluster Manager: Allocates resources across applications.
Worker Node: Slave node that is used to run the application code in the clusters.
Executor: It is a JVM process that is used to execute job in the worker nodes. 

Spark is designed to handle large-scale data processing tasks, including batch processing, stream processing, machine learning, and graph processing. It is built on top of the Hadoop Distributed File System (HDFS) and can read data from HDFS, HBase, Cassandra, and other data sources.
One of the key features of Apache Spark is its ability to process data in memory, which makes it faster than other big data processing frameworks such as Hadoop. It achieves this through the Resilient Distributed Dataset (RDD) abstraction, which allows data to be processed in parallel across a cluster of computers.
Spark provides a unified API for data processing that supports multiple programming languages such as Scala, Java, Python, and R. This makes it easy for data scientists and developers to work with large-scale data without needing to learn a new programming language or framework.
In addition to its core processing capabilities, Apache Spark also provides a range of libraries for machine learning (MLlib), graph processing (GraphX), and stream processing (Spark Streaming). These libraries provide pre-built algorithms and data structures that can be used to solve common data processing tasks.
In summary, Apache Spark is a fast and general-purpose big data processing framework that provides a unified API for data processing across a range of data processing tasks. It is designed to handle large-scale data processing tasks and supports multiple programming languages and data sources.
Apache Spark and Hadoop are two popular big data processing frameworks. While both can handle large-scale data processing, they have some differences and similarities.
Similarities:
· Both Apache Spark and Hadoop are designed to handle large-scale data processing tasks.
· They both have the ability to distribute data processing tasks across a cluster of computers, providing scalability and fault tolerance.
· They both support data processing with MapReduce programming paradigm.
· They both support data processing on HDFS (Hadoop Distributed File System).
Differences:
· Apache Spark is faster than Hadoop, as it keeps the data in memory rather than writing it to disk after each step in the processing pipeline. This makes it more suitable for real-time processing, machine learning, and interactive data analysis.
· Hadoop is designed for batch processing, while Apache Spark can handle both batch processing and real-time processing. Apache Spark provides a streamlined data processing pipeline that allows for real-time data processing with its in-memory processing capabilities.
· Apache Spark has a more extensive API than Hadoop, which makes it easier to work with complex data types and implement complex algorithms.
· Hadoop has been around for longer than Apache Spark, so it has more tools, libraries, and resources available for data processing and analytics.
In summary, while both Apache Spark and Hadoop are powerful tools for big data processing, they have different strengths and weaknesses. Hadoop is a great option for batch processing, while Apache Spark is a good choice for real-time processing and iterative data analysis.
Columnar Database
A columnar database is a database that stores data in columns rather than rows. In a row-based database, each row of data is stored together, and each row contains multiple columns of data. In a columnar database, however, each column of data is stored together, and each column contains multiple rows of data.
The advantage of a columnar database is that it can perform certain types of queries much faster than a traditional row-based database. For example, if you want to retrieve a subset of data from a particular column, you can simply read that column and ignore the others, which is much faster than reading every row of a table. Additionally, because columnar databases store data in a compressed format, they require less disk space than traditional row-based databases.
▪ A columnar database, also known as a column-oriented database
▪ A DBMS that stores data in columns rather than in rows as RDBMS
▪ The table schema defines only column families, which are the key value pairs.
▪ A table can have multiple column families and each column family can have any number of columns.
▪ Subsequent column values are stored contiguously on the disk.
▪ Each cell value of the table has a timestamp.
▪ The tables in Columnar database are sorted by row.
▪ The rows of tables in Columnar database are identified by a row-key.
▪ The difference centered around performance, storage and schema modifying techniques.
▪ Efficient write & read data to and from hard disk storage; speeds up the time it takes to return a query.
▪ Main benefits columnar operations like MIN, MAX, SUM, COUNT and AVG performed very rapidly.
▪ Columnar Database allows random read & write in Hadoop.
▪ Columnar database fulfills the ACID principles of a database.
How are Columnar Databases Used in Hadoop/Big Data Analysis?
Hadoop includes a processing framework called MapReduce. MapReduce is a programming model that allows developers to write distributed processing applications that can process large datasets in parallel across many machines.
However, MapReduce has some limitations when it comes to data processing. Specifically, it is not very efficient at processing data in a columnar format. This is because MapReduce is designed to process data in a row-based format, where each row of data is processed independently of the others. This means that if you want to perform a query that involves a single column of data, MapReduce must read every row of the dataset, which can be very slow and inefficient.
To address this limitation, many organizations are using columnar databases in conjunction with Hadoop. These databases are designed to work with Hadoop’s distributed processing framework and are optimized for columnar data storage and processing.
One example of a columnar database that is commonly used with Hadoop is Apache HBase. HBase is a distributed, column-oriented database that is designed to work with Hadoop. It is optimized for random reads and writes, which makes it well-suited for real-time data processing.
Columnar databases are becoming increasingly popular in big data analysis, especially in the context of Hadoop. They are highly efficient at processing and analyzing large volumes of data in parallel, which makes them well-suited for use cases that involve real-time data processing and analytics.
In order to use columnar databases with Hadoop, organizations need to select a database that is designed to work with Hadoop’s distributed processing framework.
Columnar databases are particularly well-suited for analytical workloads, where large amounts of data need to be processed and analyzed quickly. They are commonly used in data warehousing, business intelligence, and analytics applications, where data needs to be queried and aggregated across multiple dimensions.
However, columnar databases may not be as efficient for transactional workloads, where data is frequently updated, inserted, or deleted. In these cases, a traditional row-based database may be a better choice. Additionally, because columnar databases are a relatively new technology, they may not be as widely supported by third-party tools and applications as traditional relational databases.
Pig architecture:
Pig is a high-level platform for analyzing large data sets using a language called Pig Latin. It is a part of the Hadoop ecosystem and runs on top of Hadoop, allowing users to process and analyze large data sets in a distributed computing environment.
▪ a platform for analyzing large data sets
▪ consists of a high-level language for expressing data analysis programs
▪ coupled with infrastructure for evaluating these programs
Features:
▪ Ease of programming: It is trivial to achieve parallel execution of simple, “embarrassingly parallel” data analysis tasks Complex tasks comprised of multiple interrelated data transformations are explicitly encoded as data flow sequences Making the tasks easy to write, understand, and maintain
▪ Optimization: The way in which tasks are encoded permits the system to optimize their execution automatically, allowing the user to focus on semantics rather than efficiency
▪ Extensibility: Users can create their own functions to do special-purpose processing
What It Does Best?
▪ Join Datasets
▪ Sort Datasets
▪ Filter
▪ Group By
▪ User Defined Functions
Case Sensitivity:
• The names (aliases) of relations A, B, and C are case sensitive.
• The names (aliases) of fields f1, f2, and f3 are case sensitive.
• Function names PigStorage and COUNT are case sensitive.
• Keywords LOAD, USING, AS, GROUP, BY, FOREACH, GENERATE, and DUMP are case insensitive. They can also be written as load, using, as, group, by, etc.
• In the FOREACH statement, the field in relation B is referred to by positional notation ($0)
Pig Architecture: How it works?
▪ Pig provides an engine for executing data flows in parallel on Hadoop
▪ It includes a language, Pig Latin, for expressing these data flows
▪ Pig Latin includes operators for many of the traditional data operations (join, sort, filter, etc.), as well as the ability for users to develop their own functions for reading, processing, and writing data
▪ Pig runs on Hadoop
▪ It makes use of both the Hadoop Distributed File System, HDFS, and Hadoop’s processing system, Map-Reduce
▪ Pig Queries are written to get data stored in HDFS
▪ Pig Scripts get internally converted “INTO” MR jobs
▪ MR jobs query in turn to HDFS file system and pass back the result
Pig’s architecture is divided into two major components:
1.	Pig Latin Compiler: This component compiles Pig Latin scripts into MapReduce jobs that can run on a Hadoop cluster. The Pig Latin Compiler consists of several subcomponents, including:
•	Parser: This subcomponent is responsible for parsing the Pig Latin script and creating an abstract syntax tree (AST).
•	Optimizer: This subcomponent performs several optimization steps on the AST to optimize the execution of the Pig Latin script.
•	Compiler: Here, the output from the optimizer is compiled into a series of Map Reduce jobs and rearranges their order to optimize the performance.
•	Execution Engine: The Map Reduce jobs are then provided to Hadoop platform to produce the desired result.
2. Pig Execution Environment: This component is responsible for executing the MapReduce jobs generated by the Pig Latin Compiler. It consists of several subcomponents, including:
•	Pig Runtime: This subcomponent manages the execution of the MapReduce jobs generated by the Pig Latin Compiler.
•	Pig Engine: This subcomponent provides the interface for users to interact with the Pig system.
•	Pig Storage: This subcomponent provides the interfaces for Pig to read and write data from various data sources, including HDFS, HBase, and local file systems.
Overall, Pig’s architecture allows users to write Pig Latin scripts to process and analyze large data sets on a Hadoop cluster. The Pig Latin Compiler compiles these scripts into MapReduce jobs that can be executed on the Hadoop cluster, and the Pig Execution Environment manages the execution of these jobs.

LOAD DATA inpath 'hdfs:///myproject/output' OVERWRITE INTO TABLE insurance;
