## Twitter Sentiment Analysis

### Objective

Count the Total Number of Positive and Negative Words / Comments by,
1. processing live data streams using Spark’s streaming APIs
2. sentiment analysis of real-time tweets


### Initialization
1. 	Download and extract the twitter data zip file [here](https://drive.google.com/file/d/1K1ub__1yKOMTSNAp7f_NGiCefT6KjxXM/view?usp=sharing) and run 
`unzip 16M.txt.zip`
2. Start zookeeper service 
`$KAFKA_HOME/bin/zookeeper-server-start.sh`
`$KAFKA_HOME/config/zookeeper.properties`
3. Start Kafka service
`$KAFKA_HOME/bin/kafka-server-start.sh` `$KAFKA_HOME/config/server.properties`
4. Create topic named twitter in Kafka
`$KAFKA_HOME/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic twitterstream`
5. Check what topics you have
`$KAFKA_HOME/bin/kafka-topics.sh --list --zookeeper localhost:2181`

### Using Streaming API
1. To stream tweets, we will read tweets from a file and push them to the twitterstream topic in Kafka. Do this by running our program as follows
`$ python twitter_to_kafka.py`
<i>Note: this program must be running when you run your portion of the assignment, otherwise you will not get any tweets</i>
2. To check if the data is landing in Kafka
`$KAFKA_HOME/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic twitterstream --from-beginning`

### Functionalities
1. `positive.txt` and `negative.txt` represent the positive and negative words. 
2. `load_wordlist(filename)` : This function is used to load the positive words from positive.txt and the negative words from negative.txt. Returns words as a list.
3. `stream(ssc, pwords, nwords, duration)` :  The main streaming function that counts the positive and negative words.
4. `make_plot(counts)` : Plots the total number of positive and negative words at each time-step.

### Running the Stream Analysis Program
`$SPARK_HOME/bin/spark-submit --packages org.apache.spark:spark-streaming-kafka-0-8_2.11:2.0.0 twitterStream.py`