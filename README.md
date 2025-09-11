Hadoop MapReduce WordCount
📌 Project Overview

This project implements a WordCount application using Hadoop MapReduce on a Docker-based Hadoop cluster. It counts word frequencies in a given text dataset.

⚙️ Approach and Implementation

Mapper (WordMapper.java): Splits lines into words, filters out words <3 characters, outputs (word, 1).

Reducer (WordReducer.java): Aggregates counts for each word and outputs (word, total).

🚀 Execution Steps

Start cluster

docker compose up -d


Build project

mvn clean package


Copy files to container

docker cp target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/


Run inside container

docker exec -it resourcemanager /bin/bash
hadoop fs -mkdir -p /input/data
hadoop fs -put -f ./input.txt /input/data
hadoop jar WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/data/input.txt /output1
hadoop fs -cat /output1/*


Copy output back

hdfs dfs -get /output1 /opt/hadoop-3.2.1/share/hadoop/mapreduce/
exit
docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output1/ shared-folder/output/

⚡ Challenges & Solutions

Maven not recognized → fixed by adding bin to PATH.

HDFS output exists → removed with hadoop fs -rm -r /output1.

File not found → copied file to container before hdfs put.

📥 Input & 📤 Output

Input (input.txt)

Data science is fun
Data is the new oil
Big data and Hadoop are important
Hadoop Hadoop Hadoop


Output (part-r-00000)

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

📚 Course Info

Cloud Computing for Data Analysis
ITCS 6190/8190, Fall 2025
Instructor: Marco Vieira