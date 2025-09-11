**Hadoop MapReduce WordCount**
📌** Project Overview**

This project implements a WordCount application using Hadoop MapReduce on a Docker-based Hadoop cluster. It counts word frequencies in a given text dataset.

⚙️ Approach and Implementation

Mapper (WordMapper.java): Splits lines into words, filters out words <3 characters, outputs (word, 1).

Reducer (WordReducer.java): Aggregates counts for each word and outputs (word, total).

🚀 **Execution Steps**

**1.Start Hadoop Cluster**

docker compose up -d


**2.Build the Project with Maven**

mvn clean package


**3.This creates the JAR at:**
target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar

**4.Copy JAR and Input File to Container**

docker cp target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/


**5.Enter the ResourceManager Container**

docker exec -it resourcemanager /bin/bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/


**6.Load Input into HDFS**

hadoop fs -mkdir -p /input/data
hadoop fs -put -f ./input.txt /input/data


**7.Run the MapReduce Job**

hadoop jar WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/data/input.txt /output1


⚠️ If /output1 already exists, remove it with:

hadoop fs -rm -r /output1


**8.View Job Output in HDFS**

hadoop fs -cat /output1/*


**9.Copy Output from HDFS to Local Machine**
Inside container:

hdfs dfs -get /output1 /opt/hadoop-3.2.1/share/hadoop/mapreduce/
exit


Outside container (PowerShell):

docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output1/ shared-folder/output/


**10.Final results will be available in:**

shared-folder/output/part-r-00000

⚡ **Challenges & Solutions**

Maven not recognized → fixed by adding bin to PATH.

HDFS output exists → removed with hadoop fs -rm -r /output1.

File not found → copied file to container before hdfs put.

📥** Input & 📤 Output**

**Input (input.txt)**

Data science is fun
Data is the new oil
Big data and Hadoop are important
Hadoop Hadoop Hadoop


**Output (part-r-00000)**

Big       1
Data      2
Hadoop    3
important 1
new       1
oil       1
science   1
fun       1
data      1
and       1
are       1
the       1
