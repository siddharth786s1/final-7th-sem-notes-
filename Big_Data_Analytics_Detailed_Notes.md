# Big Data Analytics (CS-702-A)
## Comprehensive Study Notes for College Exam Preparation

---

## UNIT I: INTRODUCTION TO BIG DATA AND HADOOP

### 1. Introduction to Big Data and Hadoop

#### 1.1 What is Big Data?

**Definition**: Big Data refers to extremely large datasets that cannot be processed, stored, or analyzed using traditional data processing tools and techniques.

**Characteristics of Big Data (The 5 V's)**:

1. **Volume**: 
   - Amount of data (Terabytes, Petabytes, Exabytes)
   - Example: Facebook generates 4 petabytes of data per day
   - Traditional databases cannot handle such massive volumes

2. **Velocity**: 
   - Speed at which data is generated and processed
   - Real-time or near-real-time processing required
   - Example: Twitter processes 500 million tweets per day

3. **Variety**: 
   - Different types and formats of data
   - **Structured**: SQL databases, spreadsheets
   - **Semi-structured**: JSON, XML, log files
   - **Unstructured**: Videos, images, social media posts, emails

4. **Veracity**: 
   - Uncertainty and trustworthiness of data
   - Data quality, accuracy, and reliability issues
   - Noise and anomalies in data

5. **Value**: 
   - Extracting meaningful insights from data
   - Converting data into business value
   - ROI from Big Data investments

**Additional V's** (Extended characteristics):
- **Variability**: Inconsistency in data flow rates
- **Visualization**: Representing data in understandable formats
- **Validity**: Correctness and accuracy for intended use

#### 1.2 First Understand Cloud and Set Up an AWS Account

**Why Cloud for Big Data?**
- Scalability: Handle growing data volumes
- Cost-effective: Pay-as-you-go model
- Flexibility: Scale up/down based on demand
- Reliability: High availability and disaster recovery
- Global reach: Deploy applications worldwide

**AWS (Amazon Web Services) Overview**:
- Leading cloud service provider
- Offers 200+ services including compute, storage, database, analytics

**Key AWS Services for Big Data**:

1. **Amazon S3 (Simple Storage Service)**:
   - Object storage service
   - Scalable, durable, and secure
   - Store unlimited data
   - 99.999999999% (11 9's) durability

2. **Amazon EC2 (Elastic Compute Cloud)**:
   - Virtual servers in the cloud
   - Scalable computing capacity
   - Various instance types for different workloads

3. **Amazon EMR (Elastic MapReduce)**:
   - Managed Hadoop framework
   - Process vast amounts of data
   - Supports Spark, HBase, Presto, Flink

4. **Amazon Redshift**:
   - Data warehousing service
   - Petabyte-scale data warehouse
   - Fast query performance

5. **AWS Glue**:
   - ETL (Extract, Transform, Load) service
   - Data catalog and preparation

**Setting Up AWS Account** (Step-by-step):

1. **Create AWS Account**:
   - Visit aws.amazon.com
   - Click "Create an AWS Account"
   - Provide email, password, and account name

2. **Contact Information**:
   - Enter personal or business details
   - Choose account type (Personal/Professional)

3. **Payment Information**:
   - Add credit/debit card details
   - AWS Free Tier available for 12 months

4. **Identity Verification**:
   - Phone verification via call or SMS

5. **Choose Support Plan**:
   - Basic (Free)
   - Developer ($29/month)
   - Business ($100/month)
   - Enterprise (Custom pricing)

6. **Access AWS Management Console**:
   - Sign in with root account credentials
   - Access all AWS services

**AWS Free Tier Benefits**:
- EC2: 750 hours per month of t2.micro instances
- S3: 5GB of standard storage
- EMR: 750 hours per month
- RDS: 750 hours of db.t2.micro instances

**Best Practices for AWS**:
- Enable Multi-Factor Authentication (MFA)
- Use IAM (Identity and Access Management) for user management
- Never share root account credentials
- Set up billing alerts
- Use least privilege principle
- Enable CloudTrail for auditing

#### 1.3 Types of Digital Data

**Classification by Structure**:

**1. Structured Data**:
- **Definition**: Highly organized data with predefined schema
- **Format**: Rows and columns (tabular format)
- **Storage**: Relational databases (MySQL, PostgreSQL, Oracle)
- **Processing**: SQL queries
- **Examples**:
  - Employee records (ID, Name, Salary, Department)
  - Bank transactions
  - Inventory management systems
  - CRM data

**Advantages**:
- Easy to search and query
- Well-defined relationships
- Efficient storage and retrieval
- ACID properties (Atomicity, Consistency, Isolation, Durability)

**Disadvantages**:
- Rigid schema
- Difficult to modify structure
- Limited scalability for massive datasets

**2. Semi-Structured Data**:
- **Definition**: Data with some organizational structure but not rigid schema
- **Format**: Self-describing with tags and markers
- **Storage**: NoSQL databases, file systems
- **Examples**:
  - **JSON (JavaScript Object Notation)**:
    ```json
    {
      "name": "John Doe",
      "age": 30,
      "email": "john@example.com",
      "skills": ["Python", "Hadoop", "Spark"]
    }
    ```
  - **XML (eXtensible Markup Language)**:
    ```xml
    <employee>
      <name>John Doe</name>
      <age>30</age>
      <email>john@example.com</email>
    </employee>
    ```
  - Log files
  - CSV files with varying columns
  - Email headers

**Advantages**:
- Flexible schema
- Easy to add new fields
- Better for evolving data structures
- Human-readable

**Disadvantages**:
- More complex querying than structured data
- Requires parsing
- Larger storage overhead

**3. Unstructured Data**:
- **Definition**: Data without predefined structure or organization
- **Format**: Raw format (text, binary)
- **Storage**: Distributed file systems (HDFS), object storage (S3)
- **Constitutes**: 80-90% of all data generated

**Examples**:
- Text documents (Word, PDF)
- Emails
- Social media posts (tweets, Facebook posts)
- Images (JPEG, PNG)
- Videos (MP4, AVI)
- Audio files (MP3, WAV)
- Sensor data
- Web pages
- Medical records (X-rays, MRI scans)

**Challenges**:
- Difficult to search and analyze
- Requires specialized tools (NLP, Computer Vision)
- Massive storage requirements
- Complex processing

**Processing Techniques**:
- **Text Mining**: Extract meaningful information from text
- **Natural Language Processing (NLP)**: Understand human language
- **Computer Vision**: Analyze images and videos
- **Speech Recognition**: Convert audio to text

#### 1.4 Introduction to Big Data, Big Data Analytics

**Big Data Analytics Definition**:
Process of examining large and varied datasets to uncover hidden patterns, correlations, market trends, customer preferences, and other useful business information.

**Big Data Analytics vs Traditional Analytics**:

| Traditional Analytics | Big Data Analytics |
|----------------------|-------------------|
| Structured data only | All data types (structured, semi-structured, unstructured) |
| Sample-based analysis | Analyze entire dataset |
| Batch processing | Real-time and batch processing |
| SQL databases | Hadoop, NoSQL, distributed systems |
| Limited scalability | Highly scalable |
| Descriptive analytics | Descriptive, predictive, prescriptive |

**Types of Big Data Analytics**:

**1. Descriptive Analytics** (What happened?):
- Summarizes historical data
- Provides insights into past events
- **Techniques**: Data aggregation, data mining, visualization
- **Examples**:
  - Sales reports
  - Website traffic analysis
  - Social media engagement metrics
- **Tools**: Tableau, Power BI, Google Analytics

**2. Diagnostic Analytics** (Why did it happen?):
- Identifies causes of past events
- Drill-down analysis
- **Techniques**: Data discovery, correlation analysis
- **Examples**:
  - Why did sales decrease last quarter?
  - What caused website downtime?
  - Customer churn analysis

**3. Predictive Analytics** (What will happen?):
- Forecasts future trends and events
- Uses statistical models and machine learning
- **Techniques**: Regression, time series analysis, machine learning
- **Examples**:
  - Sales forecasting
  - Customer lifetime value prediction
  - Fraud detection
  - Weather forecasting
- **Tools**: Python (scikit-learn), R, SAS

**4. Prescriptive Analytics** (What should we do?):
- Recommends actions to achieve desired outcomes
- Optimization and simulation
- **Techniques**: Optimization algorithms, simulation, decision analysis
- **Examples**:
  - Dynamic pricing
  - Route optimization for logistics
  - Personalized recommendations
  - Resource allocation
- **Tools**: IBM Decision Optimization, Gurobi

#### 1.5 History of Hadoop

**Evolution of Hadoop**:

**Early 2000s - The Google Papers**:
- **2003**: Google publishes Google File System (GFS) paper
  - Distributed file system for large-scale data processing
  - Fault-tolerant and scalable
- **2004**: Google publishes MapReduce paper
  - Programming model for processing large datasets
  - Parallel and distributed algorithm

**Birth of Hadoop**:
- **2005**: Doug Cutting and Mike Cafarella working on Apache Nutch (open-source web search engine)
- Implemented distributed computing based on Google papers
- Named "Hadoop" after Doug Cutting's son's toy elephant

**2006**: Hadoop becomes Apache project
- Doug Cutting joins Yahoo!
- Yahoo! begins using Hadoop for web indexing

**Major Milestones**:
- **2008**: Hadoop becomes top-level Apache project
  - Yahoo! runs 10,000-node Hadoop cluster
  - Hadoop 0.20.0 released with major performance improvements
  
- **2011**: Hadoop 1.0 released
  - Stable API
  - Production-ready
  
- **2012**: Hadoop 2.0 development begins
  - YARN (Yet Another Resource Negotiator) introduced
  - Separation of resource management from MapReduce
  
- **2013**: Hadoop 2.0 released
  - YARN enables multiple processing engines
  - High Availability for NameNode
  
- **2017**: Hadoop 3.0 released
  - Erasure coding for storage efficiency
  - Multiple NameNodes for scalability
  - Support for more than 10,000 nodes

**Key Contributors**:
- Doug Cutting (Creator)
- Mike Cafarella (Co-creator)
- Yahoo! (Major early adopter and contributor)
- Apache Software Foundation

**Hadoop Ecosystem Growth**:
- Pig (2007): High-level data flow language
- Hive (2008): SQL-like query language
- HBase (2008): NoSQL database
- Spark (2010): In-memory processing engine
- Storm (2011): Real-time stream processing

#### 1.6 Apache Hadoop

**Definition**: Open-source framework for distributed storage and processing of large datasets using commodity hardware.

**Core Principles**:
1. **Scalability**: Scale from single server to thousands of machines
2. **Fault Tolerance**: Handle hardware failures automatically
3. **Data Locality**: Move computation to data (not vice versa)
4. **Open Source**: Free to use and modify

**Hadoop Architecture Components**:

**1. Hadoop Core Components**:

**A. HDFS (Hadoop Distributed File System)**:
- Distributed file system
- Stores data across multiple machines
- High throughput access to data
- Fault-tolerant through replication

**HDFS Architecture**:
```
                    NameNode (Master)
                    - Manages metadata
                    - Coordinates file operations
                         |
      +------------------+------------------+
      |                  |                  |
  DataNode 1         DataNode 2         DataNode 3
  (Slave)            (Slave)            (Slave)
  - Stores data      - Stores data      - Stores data
  - Serves read/     - Serves read/     - Serves read/
    write requests     write requests     write requests
```

**Key Features**:
- **Block-based storage**: Files divided into blocks (default 128MB)
- **Replication**: Each block replicated (default 3 times)
- **Rack awareness**: Replicas placed on different racks
- **Write-once, read-many**: Optimized for sequential reads

**B. MapReduce**:
- Programming model for parallel data processing
- Processes data in two phases: Map and Reduce
- Automatic parallelization and distribution

**C. YARN (Yet Another Resource Negotiator)**:
- Resource management layer
- Separates resource management from data processing
- Enables multiple processing engines (MapReduce, Spark, Tez)

**YARN Architecture**:
```
            ResourceManager (Master)
            - Manages cluster resources
            - Schedules applications
                     |
      +--------------+--------------+
      |                             |
  NodeManager 1                 NodeManager 2
  (Slave)                       (Slave)
  - Manages resources           - Manages resources
    on single node                on single node
  - Monitors containers         - Monitors containers
```

**2. Hadoop Ecosystem Tools**:

**Data Storage**:
- **HBase**: NoSQL database on top of HDFS
- **Cassandra**: Distributed NoSQL database

**Data Ingestion**:
- **Flume**: Collects, aggregates, and moves log data
- **Sqoop**: Transfers data between Hadoop and relational databases
- **Kafka**: Distributed streaming platform

**Data Processing**:
- **Pig**: High-level scripting language
- **Hive**: SQL-like query language
- **Spark**: Fast, in-memory processing
- **Storm**: Real-time stream processing
- **Flink**: Stream and batch processing

**Data Analysis**:
- **Mahout**: Machine learning library
- **Spark MLlib**: Machine learning on Spark

**Workflow Management**:
- **Oozie**: Workflow scheduler
- **Airflow**: Modern workflow management

**Monitoring**:
- **Ambari**: Cluster management and monitoring
- **Ganglia**: Distributed monitoring system

#### 1.7 Analyzing Data with Unix Tools

**Why Unix Tools for Data Analysis?**
- Fast and efficient for text data
- Available on all Linux/Unix systems
- Powerful when combined (piping)
- Low resource requirements
- Ideal for exploratory data analysis

**Essential Unix Commands for Data Analysis**:

**1. Basic File Operations**:

**cat** (Concatenate and display):
```bash
# Display file contents
cat data.txt

# Combine multiple files
cat file1.txt file2.txt > combined.txt

# Display with line numbers
cat -n data.txt
```

**head** (Display first lines):
```bash
# First 10 lines (default)
head data.txt

# First 20 lines
head -n 20 data.txt

# First 5 lines of multiple files
head -n 5 file1.txt file2.txt
```

**tail** (Display last lines):
```bash
# Last 10 lines
tail data.txt

# Last 50 lines
tail -n 50 data.txt

# Follow file updates (log monitoring)
tail -f /var/log/application.log
```

**2. Text Processing Commands**:

**grep** (Search patterns):
```bash
# Search for pattern
grep "error" log.txt

# Case-insensitive search
grep -i "Error" log.txt

# Count matching lines
grep -c "success" log.txt

# Display line numbers
grep -n "exception" log.txt

# Recursive search in directory
grep -r "TODO" /project/src/

# Invert match (lines NOT containing pattern)
grep -v "debug" log.txt

# Multiple patterns
grep -E "error|warning|critical" log.txt
```

**sed** (Stream editor):
```bash
# Replace first occurrence per line
sed 's/old/new/' file.txt

# Replace all occurrences
sed 's/old/new/g' file.txt

# Delete lines containing pattern
sed '/pattern/d' file.txt

# Print specific lines
sed -n '10,20p' file.txt

# In-place editing
sed -i 's/old/new/g' file.txt

# Multiple operations
sed -e 's/foo/bar/g' -e 's/baz/qux/g' file.txt
```

**awk** (Pattern scanning and processing):
```bash
# Print specific columns (space-separated)
awk '{print $1, $3}' data.txt

# Print with custom delimiter (comma)
awk -F',' '{print $1, $3}' data.csv

# Sum of column
awk '{sum += $2} END {print sum}' data.txt

# Average of column
awk '{sum += $1; count++} END {print sum/count}' numbers.txt

# Conditional printing
awk '$3 > 100 {print $1, $3}' sales.txt

# Count occurrences
awk '{count[$1]++} END {for (word in count) print word, count[word]}' words.txt

# Print lines matching pattern
awk '/error/ {print $0}' log.txt
```

**3. Sorting and Filtering**:

**sort**:
```bash
# Sort alphabetically
sort names.txt

# Sort numerically
sort -n numbers.txt

# Reverse sort
sort -r data.txt

# Sort by column (comma-separated)
sort -t',' -k2 data.csv

# Unique sort
sort -u data.txt

# Sort large files efficiently
sort --parallel=4 --buffer-size=2G large_file.txt
```

**uniq** (Remove duplicates):
```bash
# Remove adjacent duplicates (requires sorted input)
sort data.txt | uniq

# Count occurrences
sort data.txt | uniq -c

# Show only duplicates
sort data.txt | uniq -d

# Show only unique lines
sort data.txt | uniq -u
```

**4. Counting and Statistics**:

**wc** (Word count):
```bash
# Count lines, words, characters
wc file.txt

# Count only lines
wc -l file.txt

# Count only words
wc -w file.txt

# Count only bytes
wc -c file.txt

# Count lines in multiple files
wc -l *.log
```

**5. Column Operations**:

**cut**:
```bash
# Extract columns 1-3 (tab-separated)
cut -f1-3 data.tsv

# Extract columns with comma delimiter
cut -d',' -f1,3,5 data.csv

# Extract characters 1-10
cut -c1-10 file.txt
```

**paste**:
```bash
# Merge files column-wise
paste file1.txt file2.txt

# With custom delimiter
paste -d',' file1.txt file2.txt
```

**6. Advanced Text Processing**:

**tr** (Translate characters):
```bash
# Convert lowercase to uppercase
tr 'a-z' 'A-Z' < input.txt

# Delete specific characters
tr -d '[:punct:]' < text.txt

# Squeeze repeated characters
tr -s ' ' < file.txt

# Replace spaces with tabs
tr ' ' '\t' < file.txt
```

**7. Compression Tools**:

**gzip/gunzip**:
```bash
# Compress file
gzip large_file.txt

# Decompress file
gunzip large_file.txt.gz

# View compressed file
zcat file.txt.gz

# Search in compressed file
zgrep "pattern" file.txt.gz
```

**8. Piping and Combining Commands**:

**Practical Examples**:

```bash
# Example 1: Find top 10 most frequent words in file
cat book.txt | tr ' ' '\n' | sort | uniq -c | sort -rn | head -10

# Example 2: Count unique IP addresses in web server log
cat access.log | awk '{print $1}' | sort | uniq | wc -l

# Example 3: Find top 5 error types in log
grep "ERROR" app.log | awk '{print $4}' | sort | uniq -c | sort -rn | head -5

# Example 4: Calculate average response time from log
grep "response_time" api.log | awk -F'=' '{sum+=$2; count++} END {print sum/count}'

# Example 5: Extract and count HTTP status codes
awk '{print $9}' access.log | sort | uniq -c | sort -rn

# Example 6: Find files modified in last 24 hours and their sizes
find /data -type f -mtime -1 -exec ls -lh {} \; | awk '{print $9, $5}'

# Example 7: Monitor real-time error rate
tail -f app.log | grep --line-buffered "ERROR" | awk '{count++; print count " errors so far"}'
```

**Performance Tips**:
- Use `grep` before `awk` for filtering (more efficient)
- Avoid unnecessary `cat` (use input redirection)
- Use `sort -u` instead of `sort | uniq`
- For very large files, use `--parallel` options
- Use `mmap` for faster file reading when available

#### 1.8 Analyzing Data with Hadoop

**Why Hadoop for Data Analysis?**

**Limitations of Unix Tools**:
- Single machine processing
- Limited by RAM and CPU
- Cannot handle petabyte-scale data
- No fault tolerance
- Sequential processing

**Hadoop Advantages**:
- Distributed processing across clusters
- Process terabytes to petabytes of data
- Automatic fault tolerance
- Parallel processing
- Scales horizontally (add more machines)

**Hadoop Processing Model**:

**1. Data Storage in HDFS**:
```
Original File (1GB)
        |
        v
Divided into blocks (128MB each)
        |
        v
[Block 1] [Block 2] [Block 3] [Block 4] [Block 5] [Block 6] [Block 7] [Block 8]
        |
        v
Each block replicated 3 times across different DataNodes
```

**2. MapReduce Processing Paradigm**:

**Map Phase**:
- Input data split into chunks
- Each mapper processes one chunk
- Produces intermediate key-value pairs
- Runs in parallel on multiple nodes

**Shuffle and Sort Phase**:
- Groups all values by key
- Transfers data between nodes
- Prepares data for reducers

**Reduce Phase**:
- Processes grouped data
- Produces final output
- Runs in parallel on multiple nodes

**Example: Word Count in Hadoop**:

**Traditional Approach (Single Machine)**:
```bash
cat books/*.txt | tr ' ' '\n' | sort | uniq -c
```

**Hadoop MapReduce Approach**:

**Input Data**:
```
File 1: Hello World Hello
File 2: World of Hadoop
File 3: Hello Hadoop World
```

**Map Phase**:
```
Mapper 1 (File 1):
  Input: "Hello World Hello"
  Output: 
    (Hello, 1)
    (World, 1)
    (Hello, 1)

Mapper 2 (File 2):
  Input: "World of Hadoop"
  Output:
    (World, 1)
    (of, 1)
    (Hadoop, 1)

Mapper 3 (File 3):
  Input: "Hello Hadoop World"
  Output:
    (Hello, 1)
    (Hadoop, 1)
    (World, 1)
```

**Shuffle and Sort**:
```
Group by key:
  (Hadoop, [1, 1])
  (Hello, [1, 1, 1])
  (World, [1, 1, 1])
  (of, [1])
```

**Reduce Phase**:
```
Reducer 1:
  Input: (Hadoop, [1, 1])
  Output: (Hadoop, 2)

Reducer 2:
  Input: (Hello, [1, 1, 1])
  Output: (Hello, 3)

Reducer 3:
  Input: (World, [1, 1, 1])
  Output: (World, 3)

Reducer 4:
  Input: (of, [1])
  Output: (of, 1)
```

**Final Output**:
```
Hadoop  2
Hello   3
World   3
of      1
```

**MapReduce Code Example (Java)**:

```java
// Mapper Class
public class WordCountMapper extends Mapper<LongWritable, Text, Text, IntWritable> {
    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();
    
    public void map(LongWritable key, Text value, Context context) 
            throws IOException, InterruptedException {
        String line = value.toString();
        String[] words = line.split("\\s+");
        
        for (String w : words) {
            word.set(w);
            context.write(word, one);
        }
    }
}

// Reducer Class
public class WordCountReducer extends Reducer<Text, IntWritable, Text, IntWritable> {
    public void reduce(Text key, Iterable<IntWritable> values, Context context)
            throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
            sum += val.get();
        }
        context.write(key, new IntWritable(sum));
    }
}

// Driver Class
public class WordCount {
    public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf, "word count");
        
        job.setJarByClass(WordCount.class);
        job.setMapperClass(WordCountMapper.class);
        job.setCombinerClass(WordCountReducer.class);
        job.setReducerClass(WordCountReducer.class);
        
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        
        FileInputFormat.addInputPath(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

**Running Hadoop Job**:
```bash
# Compile Java code
javac -classpath $(hadoop classpath) WordCount.java

# Create JAR file
jar cf wordcount.jar WordCount*.class

# Upload data to HDFS
hadoop fs -put input.txt /user/hadoop/input/

# Run MapReduce job
hadoop jar wordcount.jar WordCount /user/hadoop/input /user/hadoop/output

# View results
hadoop fs -cat /user/hadoop/output/part-r-00000
```

**Hadoop vs Unix Tools Comparison**:

| Aspect | Unix Tools | Hadoop |
|--------|-----------|---------|
| Data Size | Up to GBs | TBs to PBs |
| Processing | Single machine | Distributed cluster |
| Speed (small data) | Very fast | Slower (overhead) |
| Speed (big data) | Bottlenecked | Scales linearly |
| Fault Tolerance | None | Automatic |
| Complexity | Simple | More complex setup |
| Cost | Free | Infrastructure cost |

**When to Use Each**:

**Use Unix Tools when**:
- Data fits on single machine
- Quick exploratory analysis
- Simple text processing
- Development and testing
- Interactive analysis

**Use Hadoop when**:
- Data exceeds single machine capacity
- Need fault tolerance
- Parallel processing required
- Batch processing of large datasets
- Long-running production jobs

#### 1.9 Hadoop Streaming

**Definition**: Hadoop Streaming is a utility that allows users to create and run MapReduce jobs with any executable or script as the mapper and/or reducer.

**Key Features**:
- Write MapReduce jobs in any language (Python, Ruby, Perl, Shell scripts)
- No need to learn Java
- Standard input/output for communication
- Easy to develop and test locally

**How Hadoop Streaming Works**:
1. Mapper reads input from standard input (stdin)
2. Mapper writes output to standard output (stdout)
3. Hadoop framework handles data distribution
4. Reducer reads input from stdin
5. Reducer writes final output to stdout

**Python Example - Word Count**:

**Mapper (mapper.py)**:
```python
#!/usr/bin/env python3
import sys

# Read input line by line from stdin
for line in sys.stdin:
    # Remove leading/trailing whitespace
    line = line.strip()
    
    # Split line into words
    words = line.split()
    
    # Emit each word with count 1
    for word in words:
        # Write to stdout
        print(f"{word}\t1")
```

**Reducer (reducer.py)**:
```python
#!/usr/bin/env python3
import sys

current_word = None
current_count = 0

# Read input from mapper
for line in sys.stdin:
    # Remove whitespace
    line = line.strip()
    
    # Parse input (word and count separated by tab)
    word, count = line.split('\t')
    count = int(count)
    
    # If same word, accumulate count
    if word == current_word:
        current_count += count
    else:
        # New word encountered
        if current_word:
            # Output previous word and its total count
            print(f"{current_word}\t{current_count}")
        current_word = word
        current_count = count

# Output last word
if current_word:
    print(f"{current_word}\t{current_count}")
```

**Local Testing (before Hadoop)**:
```bash
# Make scripts executable
chmod +x mapper.py reducer.py

# Test pipeline locally
cat input.txt | ./mapper.py | sort | ./reducer.py
```

**Running on Hadoop**:
```bash
# Put input file on HDFS
hadoop fs -put input.txt /user/hadoop/input/

# Run Hadoop Streaming job
hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-*.jar \
    -input /user/hadoop/input/input.txt \
    -output /user/hadoop/output \
    -mapper mapper.py \
    -reducer reducer.py \
    -file mapper.py \
    -file reducer.py

# View results
hadoop fs -cat /user/hadoop/output/part-00000
```

**Advanced Streaming Example - Log Analysis**:

**Mapper (log_mapper.py)**:
```python
#!/usr/bin/env python3
import sys
import re
from datetime import datetime

# Apache log pattern
log_pattern = r'(\S+) - - \[(.*?)\] "(\S+) (\S+) (\S+)" (\d+) (\d+)'

for line in sys.stdin:
    match = re.match(log_pattern, line)
    if match:
        ip = match.group(1)
        timestamp = match.group(2)
        method = match.group(3)
        url = match.group(4)
        status = match.group(6)
        size = match.group(7)
        
        # Extract hour from timestamp
        dt = datetime.strptime(timestamp, '%d/%b/%Y:%H:%M:%S %z')
        hour = dt.strftime('%Y-%m-%d %H:00')
        
        # Emit: hour, status, size
        print(f"{hour}\t{status}\t{size}")
```

**Reducer (log_reducer.py)**:
```python
#!/usr/bin/env python3
import sys
from collections import defaultdict

hour_stats = defaultdict(lambda: {'count': 0, 'total_size': 0, 'status_codes': defaultdict(int)})

for line in sys.stdin:
    hour, status, size = line.strip().split('\t')
    size = int(size)
    
    hour_stats[hour]['count'] += 1
    hour_stats[hour]['total_size'] += size
    hour_stats[hour]['status_codes'][status] += 1

# Output statistics
for hour, stats in sorted(hour_stats.items()):
    avg_size = stats['total_size'] / stats['count']
    print(f"{hour}\tRequests:{stats['count']}\tAvgSize:{avg_size:.2f}")
    for status, count in stats['status_codes'].items():
        print(f"{hour}\tStatus{status}:{count}")
```

**Hadoop Streaming with Combiner**:
```bash
hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-*.jar \
    -input /logs/access.log \
    -output /logs/analysis \
    -mapper mapper.py \
    -reducer reducer.py \
    -combiner reducer.py \
    -file mapper.py \
    -file reducer.py
```

**Advantages of Hadoop Streaming**:
- Language flexibility
- Easy development and testing
- Familiar programming environment
- Quick prototyping
- Utilize existing scripts and tools

**Limitations**:
- Performance overhead (compared to native Java)
- Limited access to Hadoop API features
- Serialization/deserialization overhead
- Not suitable for complex custom input formats

#### 1.10 Hadoop Echo System

**Definition**: Collection of tools and frameworks that work with or on top of Hadoop to provide additional functionality.

**Complete Hadoop Ecosystem Overview**:

```
                    Hadoop Ecosystem
                           |
        +------------------+------------------+
        |                                     |
   Core Hadoop                        Ecosystem Tools
        |                                     |
   +----+----+                    +-----------+-----------+
   |    |    |                    |           |           |
 HDFS YARN MapReduce          Processing   Storage   Integration
                                   |           |           |
                            +------+-----+  +--+---+  +----+----+
                            |      |     |  |      |  |    |    |
                          Pig  Hive Spark HBase Kudu Sqoop Flume Kafka
```

**Detailed Ecosystem Components**:

**1. Core Hadoop**:
- **HDFS**: Distributed storage
- **MapReduce**: Batch processing framework
- **YARN**: Resource management

**2. Data Storage**:

**HBase**:
- NoSQL database built on HDFS
- Column-oriented storage
- Real-time read/write access
- Billions of rows × millions of columns
- **Use Cases**: Time-series data, sensor data, real-time analytics

**Kudu**:
- Fast analytics on fast data
- Columnar storage
- Low-latency random access
- **Use Cases**: Real-time analytics, time-series workloads

**3. Data Processing**:

**Apache Pig**:
- High-level platform for creating MapReduce programs
- Pig Latin scripting language
- Translates scripts to MapReduce jobs
- **Advantages**: Less code, easier to learn than Java

**Example Pig Script**:
```pig
-- Load data
logs = LOAD '/logs/access.log' USING PigStorage('\t') 
       AS (ip:chararray, timestamp:chararray, url:chararray, status:int);

-- Filter successful requests
successful = FILTER logs BY status == 200;

-- Group by IP
grouped = GROUP successful BY ip;

-- Count requests per IP
counts = FOREACH grouped GENERATE group AS ip, COUNT(successful) AS count;

-- Sort by count
sorted = ORDER counts BY count DESC;

-- Get top 10
top10 = LIMIT sorted 10;

-- Store results
STORE top10 INTO '/output/top_ips';
```

**Apache Hive**:
- Data warehouse infrastructure
- SQL-like query language (HiveQL)
- Converts SQL to MapReduce/Tez/Spark jobs
- Schema on read
- **Use Cases**: Data warehousing, batch analytics

**Example Hive Queries**:
```sql
-- Create table
CREATE TABLE employees (
    id INT,
    name STRING,
    salary FLOAT,
    department STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE;

-- Load data
LOAD DATA INPATH '/data/employees.csv' INTO TABLE employees;

-- Query data
SELECT department, AVG(salary) as avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000
ORDER BY avg_salary DESC;

-- Join tables
SELECT e.name, e.salary, d.dept_name
FROM employees e
JOIN departments d ON e.department = d.dept_id
WHERE e.salary > 60000;
```

**Apache Spark**:
- Fast, in-memory processing engine
- 100x faster than MapReduce for certain workloads
- Supports batch, streaming, machine learning, graph processing
- APIs in Java, Scala, Python, R
- **Components**:
  - Spark SQL: Structured data processing
  - Spark Streaming: Real-time stream processing
  - MLlib: Machine learning library
  - GraphX: Graph computation

**Example Spark (Python)**:
```python
from pyspark import SparkContext, SparkConf

# Initialize Spark
conf = SparkConf().setAppName("WordCount")
sc = SparkContext(conf=conf)

# Read data
text_file = sc.textFile("hdfs:///input/data.txt")

# Process data
word_counts = (text_file
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b))

# Save results
word_counts.saveAsTextFile("hdfs:///output/wordcount")
```

**Apache Storm**:
- Real-time stream processing
- Processes unbounded streams of data
- Low latency
- **Use Cases**: Real-time analytics, ETL, continuous computation

**Apache Flink**:
- Stream and batch processing
- True streaming (not micro-batching)
- Event time processing
- Exactly-once semantics

**4. Data Integration**:

**Apache Sqoop**:
- Transfers data between Hadoop and relational databases
- Bulk import/export
- Supports MySQL, PostgreSQL, Oracle, SQL Server

**Import from Database to HDFS**:
```bash
# Import entire table
sqoop import \
    --connect jdbc:mysql://localhost/employees \
    --username root \
    --password password \
    --table employees \
    --target-dir /user/hadoop/employees

# Import with query
sqoop import \
    --connect jdbc:mysql://localhost/sales \
    --username root \
    --password password \
    --query 'SELECT * FROM orders WHERE $CONDITIONS AND year=2023' \
    --target-dir /user/hadoop/sales_2023 \
    --split-by order_id

# Import directly to Hive
sqoop import \
    --connect jdbc:mysql://localhost/hr \
    --username root \
    --password password \
    --table employees \
    --hive-import \
    --hive-table hr.employees
```

**Export from HDFS to Database**:
```bash
sqoop export \
    --connect jdbc:mysql://localhost/results \
    --username root \
    --password password \
    --table analysis_results \
    --export-dir /user/hadoop/output/results
```

**Apache Flume**:
- Collects, aggregates, and moves large amounts of log data
- Distributed, reliable, and available
- **Architecture**:
  - Source: Where data originates (files, syslog, HTTP)
  - Channel: Buffer between source and sink
  - Sink: Destination (HDFS, HBase, Kafka)

**Flume Configuration Example**:
```properties
# Define source, channel, and sink
agent.sources = weblog
agent.channels = memChannel
agent.sinks = hdfsSink

# Configure source
agent.sources.weblog.type = exec
agent.sources.weblog.command = tail -F /var/log/apache/access.log

# Configure channel
agent.channels.memChannel.type = memory
agent.channels.memChannel.capacity = 1000

# Configure sink
agent.sinks.hdfsSink.type = hdfs
agent.sinks.hdfsSink.hdfs.path = hdfs://namenode/logs/%Y/%m/%d
agent.sinks.hdfsSink.hdfs.fileType = DataStream

# Bind source and sink to channel
agent.sources.weblog.channels = memChannel
agent.sinks.hdfsSink.channel = memChannel
```

**Apache Kafka**:
- Distributed streaming platform
- Publish-subscribe messaging system
- High throughput, low latency
- Durable message storage
- **Use Cases**: Log aggregation, stream processing, event sourcing

**Kafka Components**:
- **Producer**: Publishes messages to topics
- **Consumer**: Subscribes to topics and processes messages
- **Broker**: Kafka server that stores messages
- **Topic**: Category/feed name to which messages are published
- **Partition**: Topics divided into partitions for parallelism

**5. Workflow Management**:

**Apache Oozie**:
- Workflow scheduler for Hadoop jobs
- Manages dependencies between jobs
- Supports MapReduce, Pig, Hive, Sqoop, Java actions
- Time-based and data-based triggers

**Example Oozie Workflow**:
```xml
<workflow-app name="data-pipeline" xmlns="uri:oozie:workflow:0.5">
    <start to="sqoop-import"/>
    
    <action name="sqoop-import">
        <sqoop xmlns="uri:oozie:sqoop-action:0.4">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <command>import --connect jdbc:mysql://localhost/db --table data</command>
        </sqoop>
        <ok to="hive-analysis"/>
        <error to="fail"/>
    </action>
    
    <action name="hive-analysis">
        <hive xmlns="uri:oozie:hive-action:0.5">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <script>analysis.hql</script>
        </hive>
        <ok to="end"/>
        <error to="fail"/>
    </action>
    
    <kill name="fail">
        <message>Workflow failed, error message[${wf:errorMessage(wf:lastErrorNode())}]</message>
    </kill>
    
    <end name="end"/>
</workflow-app>
```

**Apache Airflow** (Modern alternative):
- Python-based workflow management
- DAG (Directed Acyclic Graph) based
- Rich UI for monitoring
- Extensible with operators

**6. Monitoring and Management**:

**Apache Ambari**:
- Web-based management and monitoring
- Cluster provisioning
- Configuration management
- Alerts and notifications
- Service management (start/stop/restart)

**Ganglia**:
- Distributed monitoring system
- Metrics collection and visualization
- Scalable to thousands of nodes

**7. Machine Learning**:

**Apache Mahout**:
- Scalable machine learning library
- Algorithms for classification, clustering, recommendation
- Originally MapReduce-based, now Spark-based

**Spark MLlib**:
- Spark's machine learning library
- Classification, regression, clustering, collaborative filtering
- Feature extraction and transformation
- Model evaluation and tuning

**8. Data Query and Analysis**:

**Presto**:
- Distributed SQL query engine
- Query data where it lives (HDFS, S3, databases)
- Low latency, interactive queries
- Developed by Facebook

**Apache Drill**:
- Schema-free SQL query engine
- Query nested data (JSON, Parquet)
- No schema definition required

**Impala** (Cloudera):
- Massive Parallel Processing (MPP) SQL query engine
- Low-latency queries on HDFS and HBase
- Uses own daemons (not MapReduce)

**9. Data Governance and Security**:

**Apache Atlas**:
- Data governance and metadata management
- Data lineage tracking
- Data classification and discovery

**Apache Ranger**:
- Centralized security framework
- Fine-grained access control
- Audit trails
- Policies for Hadoop services

**Apache Knox**:
- REST API Gateway for Hadoop
- Single point of authentication
- SSL/TLS encryption

**10. Serialization**:

**Apache Avro**:
- Data serialization system
- Compact binary format
- Schema evolution support
- Rich data structures

**Apache Parquet**:
- Columnar storage format
- Efficient compression
- Optimized for analytics
- Works with Spark, Hive, Impala

**Apache ORC** (Optimized Row Columnar):
- Columnar storage format
- High compression
- Built-in indexes
- Optimized for Hive

**Ecosystem Integration Example**:

**Complete Data Pipeline**:
```
Web Server Logs
      ↓
   Flume (collect logs)
      ↓
   Kafka (buffer/stream)
      ↓
   Spark Streaming (real-time processing)
      ↓
   HBase (store results)
      ↓
   Hive (batch analytics)
      ↓
   Spark MLlib (machine learning)
      ↓
   Visualization Tools (Tableau, Power BI)
```

---

## UNIT II: BIG DATA STRATEGY AND DESIGN

### 1. IBM Big Data Strategy

**IBM's Approach to Big Data**:

IBM recognizes that Big Data is not just about technology, but a complete strategy involving people, processes, and technology.

**IBM Big Data Strategy Pillars**:

**1. Data as an Asset**:
- Treat data as valuable organizational resource
- Data-driven decision making
- Monetize data assets

**2. Analytics-Driven Insights**:
- Descriptive, predictive, and prescriptive analytics
- Real-time and batch analytics
- Machine learning and AI integration

**3. Enterprise-Wide Adoption**:
- Democratize data access
- Self-service analytics
- Data literacy programs

**4. Scalable Infrastructure**:
- Cloud and on-premises solutions
- Hybrid cloud architecture
- Flexible deployment options

**IBM Big Data Platform Components**:

**IBM InfoSphere BigInsights**:
- Hadoop-based analytics platform
- Enterprise-grade security and governance
- SQL access through Big SQL
- Text analytics capabilities

**IBM Streams**:
- Real-time stream processing
- Low-latency analytics
- Complex event processing

**IBM Watson**:
- Cognitive computing platform
- Natural language processing
- Machine learning and AI
- Industry-specific solutions

**IBM Cloudant**:
- NoSQL database service
- JSON document store
- Distributed architecture
- Offline-first sync

**IBM Big Data Architecture**:
```
                     Data Sources
                          |
    +---------------------+---------------------+
    |                     |                     |
Operational Data    Social Media         Sensor/IoT Data
    |                     |                     |
    +---------------------+---------------------+
                          |
                   Data Ingestion
              (Flume, Kafka, Streams)
                          |
                   Data Storage
              (HDFS, HBase, Cloudant)
                          |
                   Data Processing
          (Hadoop, Spark, Streams, Watson)
                          |
                   Data Analysis
        (BigInsights, SPSS, Cognos Analytics)
                          |
                 Data Visualization
           (Cognos, Watson Analytics, Tableau)
                          |
               Business Insights & Actions
```

**IBM Big Data Maturity Model**:

**Level 1 - Education**:
- Understanding Big Data potential
- Identifying use cases
- Building awareness

**Level 2 - Exploration**:
- Proof of concept projects
- Experimentation with tools
- Pilot implementations

**Level 3 - Engagement**:
- Production deployments
- Cross-functional teams
- Measurable business outcomes

**Level 4 - Embed**:
- Enterprise-wide adoption
- Data-driven culture
- Continuous innovation

**Level 5 - Execute**:
- Strategic competitive advantage
- Optimization and excellence
- Industry leadership

**IBM Big Data Use Cases**:

**1. Customer 360 View**:
- Integrate customer data from all touchpoints
- Personalized marketing
- Improved customer experience

**2. Fraud Detection**:
- Real-time transaction analysis
- Anomaly detection
- Pattern recognition

**3. Predictive Maintenance**:
- Sensor data analysis
- Failure prediction
- Optimized maintenance scheduling

**4. Supply Chain Optimization**:
- Demand forecasting
- Inventory optimization
- Route optimization

**5. Risk Management**:
- Credit risk assessment
- Operational risk monitoring
- Compliance monitoring

#### 2. Introduction to MapReduce

**What is MapReduce?**

MapReduce is a programming model and associated implementation for processing and generating large datasets with a parallel, distributed algorithm on a cluster.

**History**:
- Developed by Google (2004)
- Paper published by Jeffrey Dean and Sanjay Ghemawat
- Inspired Hadoop MapReduce implementation

**MapReduce Philosophy**:
- "Move computation to data" (not vice versa)
- Automatic parallelization
- Fault tolerance built-in
- Abstraction hides complexity

**MapReduce Programming Model**:

**Key Concepts**:
1. **Input**: Dataset split into independent chunks
2. **Map**: Process each chunk independently
3. **Shuffle & Sort**: Group intermediate results by key
4. **Reduce**: Aggregate grouped data
5. **Output**: Final results

**Data Flow**:
```
Input Data
    ↓
Split into chunks
    ↓
+---+---+---+
|   |   |   |
Map Map Map  (Parallel)
|   |   |   |
+---+---+---+
    ↓
Intermediate Key-Value Pairs
    ↓
Shuffle & Sort (Group by Key)
    ↓
+---+---+
|   |   |
Reduce Reduce (Parallel)
|   |   |
+---+---+
    ↓
Final Output
```

**MapReduce Phases in Detail**:

**1. Input Split**:
- Input data divided into fixed-size pieces (splits)
- Typically aligned with HDFS block size (128MB)
- Each split processed by one mapper

**2. Map Phase**:
- User-defined map function
- Input: Key-value pair
- Processing: Custom logic
- Output: Intermediate key-value pairs
- **Example**: (line_number, "hello world") → ("hello", 1), ("world", 1)

**3. Shuffle Phase** (Framework handles automatically):
- **Partition**: Assign intermediate keys to reducers
- **Sort**: Sort data by key
- **Transfer**: Move data from mappers to reducers over network
- **Merge**: Combine data from multiple mappers

**4. Reduce Phase**:
- User-defined reduce function
- Input: Key and list of values
- Processing: Aggregation logic
- Output: Final key-value pairs
- **Example**: ("hello", [1, 1, 1]) → ("hello", 3)

**5. Output**:
- Results written to HDFS
- One output file per reducer (part-r-00000, part-r-00001, etc.)

**MapReduce Word Count Example (Detailed)**:

**Input File**:
```
Hello World
Hello Hadoop
Hadoop is great
```

**Step 1: Input Split**:
```
Split 1: "Hello World"
Split 2: "Hello Hadoop"
Split 3: "Hadoop is great"
```

**Step 2: Map Phase**:
```
Mapper 1 (Split 1):
Input:  (0, "Hello World")
Output: ("Hello", 1)
        ("World", 1)

Mapper 2 (Split 2):
Input:  (0, "Hello Hadoop")
Output: ("Hello", 1)
        ("Hadoop", 1)

Mapper 3 (Split 3):
Input:  (0, "Hadoop is great")
Output: ("Hadoop", 1)
        ("is", 1)
        ("great", 1)
```

**Step 3: Shuffle & Sort**:
```
Partitioned and Sorted:
("great", [1])
("Hadoop", [1, 1])
("Hello", [1, 1])
("is", [1])
("World", [1])
```

**Step 4: Reduce Phase**:
```
Reducer 1:
Input:  ("great", [1])
Output: ("great", 1)

Input:  ("Hadoop", [1, 1])
Output: ("Hadoop", 2)

Reducer 2:
Input:  ("Hello", [1, 1])
Output: ("Hello", 2)

Input:  ("is", [1])
Output: ("is", 1)

Input:  ("World", [1])
Output: ("World", 1)
```

**Step 5: Final Output**:
```
great   1
Hadoop  2
Hello   2
is      1
World   1
```

**MapReduce Data Types**:

**Input/Output Formats**:
- **TextInputFormat**: Plain text files (default)
- **KeyValueTextInputFormat**: Tab-separated key-value pairs
- **SequenceFileInputFormat**: Binary format
- **DBInputFormat**: Read from databases
- **Custom InputFormats**: User-defined

**Writable Types** (Hadoop's serialization):
- IntWritable: Integer
- LongWritable: Long integer
- FloatWritable: Floating point
- DoubleWritable: Double precision
- Text: String (UTF-8)
- BooleanWritable: Boolean
- NullWritable: Null value

**MapReduce Job Configuration**:

```java
Configuration conf = new Configuration();
Job job = Job.getInstance(conf, "Job Name");

// Set JAR class
job.setJarByClass(DriverClass.class);

// Set Mapper and Reducer classes
job.setMapperClass(MyMapper.class);
job.setReducerClass(MyReducer.class);

// Set output key-value types
job.setOutputKeyClass(Text.class);
job.setOutputValueClass(IntWritable.class);

// Set input/output formats
job.setInputFormatClass(TextInputFormat.class);
job.setOutputFormatClass(TextOutputFormat.class);

// Set input/output paths
FileInputFormat.addInputPath(job, new Path(args[0]));
FileOutputFormat.setOutputPath(job, new Path(args[1]));

// Set number of reducers
job.setNumReduceTasks(2);

// Optional: Set Combiner (mini-reducer on mapper side)
job.setCombinerClass(MyReducer.class);

// Submit job
System.exit(job.waitForCompletion(true) ? 0 : 1);
```

**MapReduce Optimization Techniques**:

**1. Combiner**:
- Mini-reducer runs on mapper node
- Reduces data transferred to reducers
- Must be associative and commutative
- **Example**: Sum can use combiner, but average cannot directly

**2. Partitioner**:
- Controls which reducer gets which keys
- Default: Hash-based partitioning
- Custom partitioner for load balancing

**3. Compression**:
- Compress intermediate data
- Reduces network transfer
- Trade-off: CPU vs I/O

**4. In-Mapper Combining**:
- Aggregate in mapper before emitting
- Reduces key-value pairs emitted

**MapReduce Limitations**:
- Not suitable for iterative algorithms (machine learning)
- High latency (disk I/O)
- Not ideal for real-time processing
- Overkill for simple tasks
- Steep learning curve

**Alternatives to MapReduce**:
- **Apache Spark**: In-memory processing
- **Apache Tez**: DAG-based execution
- **Apache Flink**: Stream and batch processing

#### 3. Apache Spark

**What is Apache Spark?**

Apache Spark is a unified analytics engine for large-scale data processing, with built-in modules for streaming, SQL, machine learning, and graph processing.

**Spark History**:
- Developed at UC Berkeley AMPLab (2009)
- Open-sourced (2010)
- Apache top-level project (2014)
- Created by Matei Zaharia

**Spark vs MapReduce**:

| Feature | MapReduce | Spark |
|---------|-----------|-------|
| Processing Speed | Slower (disk-based) | Up to 100x faster (in-memory) |
| Ease of Use | More code required | Concise, high-level APIs |
| Real-time Processing | Not designed for | Excellent support |
| Iterative Algorithms | Inefficient | Highly efficient |
| Machine Learning | Limited (Mahout) | Built-in MLlib |
| Graph Processing | Not native | Built-in GraphX |
| Languages | Primarily Java | Java, Scala, Python, R, SQL |

**Spark Architecture**:

```
           Spark Application
                  |
         +--------+--------+
         |                 |
    Driver Program    Cluster Manager
         |           (YARN/Mesos/Standalone)
    SparkContext           |
         |                 |
         +--------+--------+
                  |
        +---------+---------+
        |         |         |
    Executor  Executor  Executor
    (Worker)  (Worker)  (Worker)
        |         |         |
     Tasks     Tasks     Tasks
```

**Components**:
- **Driver Program**: Runs main() function, creates SparkContext
- **SparkContext**: Coordinates execution, connects to cluster
- **Cluster Manager**: Allocates resources
- **Executor**: Runs tasks, stores data for caching
- **Task**: Unit of work sent to executor

**Spark Core Abstraction: RDD**:

**RDD (Resilient Distributed Dataset)**:
- Immutable distributed collection of objects
- Partitioned across cluster
- Can be cached in memory
- Fault-tolerant (lineage-based recovery)

**RDD Creation**:
```python
# From collection
data = [1, 2, 3, 4, 5]
rdd = sc.parallelize(data)

# From external source
rdd = sc.textFile("hdfs:///path/to/file.txt")

# From another RDD
mapped_rdd = rdd.map(lambda x: x * 2)
```

**RDD Operations**:

**1. Transformations** (Lazy evaluation):
- Create new RDD from existing one
- Not executed immediately
- Built computation DAG

**Common Transformations**:
```python
# map: Apply function to each element
rdd2 = rdd1.map(lambda x: x * 2)

# filter: Keep elements matching condition
rdd2 = rdd1.filter(lambda x: x > 10)

# flatMap: Map and flatten results
rdd2 = rdd1.flatMap(lambda x: x.split())

# distinct: Remove duplicates
rdd2 = rdd1.distinct()

# union: Combine two RDDs
rdd3 = rdd1.union(rdd2)

# intersection: Common elements
rdd3 = rdd1.intersection(rdd2)

# subtract: Elements in rdd1 but not rdd2
rdd3 = rdd1.subtract(rdd2)

# groupByKey: Group values by key
rdd2 = rdd1.groupByKey()

# reduceByKey: Aggregate values by key
rdd2 = rdd1.reduceByKey(lambda a, b: a + b)

# sortByKey: Sort by key
rdd2 = rdd1.sortByKey()

# join: Join two RDDs by key
rdd3 = rdd1.join(rdd2)
```

**2. Actions** (Trigger execution):
- Return value to driver or write to storage
- Trigger execution of transformation DAG

**Common Actions**:
```python
# collect: Return all elements to driver
result = rdd.collect()

# count: Count number of elements
num = rdd.count()

# first: Return first element
element = rdd.first()

# take: Return first n elements
elements = rdd.take(10)

# top: Return top n elements
elements = rdd.top(5)

# reduce: Aggregate elements
sum = rdd.reduce(lambda a, b: a + b)

# foreach: Apply function to each element (side effects)
rdd.foreach(lambda x: print(x))

# saveAsTextFile: Write to file
rdd.saveAsTextFile("hdfs:///output/path")

# countByKey: Count occurrences of each key
counts = rdd.countByKey()
```

**Spark Word Count Example**:

```python
from pyspark import SparkContext, SparkConf

# Initialize Spark
conf = SparkConf().setAppName("WordCount").setMaster("local[*]")
sc = SparkContext(conf=conf)

# Read text file
text_file = sc.textFile("input.txt")

# Word count
word_counts = (text_file
    .flatMap(lambda line: line.split())      # Split lines into words
    .map(lambda word: (word, 1))             # Map each word to (word, 1)
    .reduceByKey(lambda a, b: a + b))        # Sum counts per word

# Collect and print results
for word, count in word_counts.collect():
    print(f"{word}: {count}")

# Or save to file
word_counts.saveAsTextFile("output")

# Stop Spark
sc.stop()
```

**Spark Components**:

**1. Spark SQL**:
- Structured data processing
- SQL queries on DataFrames
- Integrates with Hive

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("SparkSQL").getOrCreate()

# Read JSON
df = spark.read.json("people.json")

# Show DataFrame
df.show()

# SQL query
df.createOrReplaceTempView("people")
result = spark.sql("SELECT name, age FROM people WHERE age > 21")
result.show()

# DataFrame API
result = df.filter(df.age > 21).select("name", "age")
```

**2. Spark Streaming**:
- Real-time stream processing
- Micro-batch processing model
- Integrates with Kafka, Flume, Kinesis

```python
from pyspark.streaming import StreamingContext

# Create StreamingContext (1-second batches)
ssc = StreamingContext(sc, 1)

# Create DStream from socket
lines = ssc.socketTextStream("localhost", 9999)

# Process stream
word_counts = (lines
    .flatMap(lambda line: line.split())
    .map(lambda word: (word, 1))
    .reduceByKey(lambda a, b: a + b))

word_counts.pprint()

# Start streaming
ssc.start()
ssc.awaitTermination()
```

**3. MLlib (Machine Learning Library)**:
- Classification, regression, clustering
- Collaborative filtering
- Dimensionality reduction
- Feature extraction

```python
from pyspark.ml.classification import LogisticRegression
from pyspark.ml.feature import VectorAssembler

# Load data
df = spark.read.csv