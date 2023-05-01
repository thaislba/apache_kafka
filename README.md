# apache_kafka

This repository is an exercise based on the DEMOS proposed in the course https://app.pluralsight.com/library/courses/apache-kafka-getting-started/table-of-contents. Everything will be done using the default configurations.

### Pre-requisites:
- Linux
- Java 8 JDK
- Scala 2.11.x


### Usage:

**1. start zookeeper with default configuration**

Run:
````
cd kafka_2.13-3.4.0
bin/zookeeper-server-start.sh config/zookeeper.properties
````
Check if zookeeper is running:

````
telnet localhost 2181
````
type: stat

**2. start kafka**

Run: 
````
bin/kafka-server-start.sh config/server.properties
````

**3. start a topic**

Run
````
bin/kafka-topics.sh
````

to get information about the parameters that has to be passesand then create your topic following the example:

````
bin/kafka-topics.sh --create --topic my_topic --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1
````
Check if everything went well:

````
cd /tmp/kafka-logs/
ls

````

**4. produce and consume messages**
To strt producing messages it is important to call a shell responsible for that: 
````
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic my_topic
````
an input terminal will be available and everything typed followed by an enter will become a message that will be sent to the broker.

To read the messages, run:
````
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my_topic

````
If you use multiple terminals, you're gonna be able to see in the consumer the new messages types in the producer.
