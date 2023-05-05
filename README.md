# apache_kafka

This repository is an exercise based on the DEMOS proposed in the course https://app.pluralsight.com/library/courses/apache-kafka-getting-started/table-of-contents. The goal is to get acquainted with Kafka's configuration files, its shell scripts and be able to run a broker locally. In the future there will be other directories in this repo with the intention of creating a better environment for creating and consuming messages. 

## Pre-requisites:
- Linux
- Java 8 JDK
- Scala 2.11.x


## Running one broker
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
bin/kafka-topics.sh 
--create --topic my_topic 
--bootstrap-server localhost:9092 
--replication-factor 1 
--partitions 1
````
Check if everything went well:

````
cd /tmp/kafka-logs/
ls
````

**4. produce and consume messages**
To start producing messages it is important to call a shell responsible for that: 
````
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic my_topic
````
an input terminal will be available and everything typed followed by an enter will become a message that will be sent to the broker.

To read the messages, run:
````
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my_topic
````
If you use multiple terminals, you're gonna be able to see in the consumer the new messages types in the producer.

## Running multiple brokers in a single machine

Check the official documentation: https://kafka.apache.org/quickstart#quickstart_multibroker

Navigate to the folder config (`cd config/`) and examine the files. You'll notice there's a file called `server.properties` which is a configuration file necessary for specifying different settings and configurations for a Kafka broker (`cat server.properties`). 
To create multiple brokers it's necessary to replicate that file such as: `server-0.properties`, `server-1.properties`, `server-3.properties` and so on.

Attention: remember you need to have an instance of zookeeper running for each broker. 


**1. Initiate the zookeeper's instances**
(not sure if it's really necessary to make a copy, still figuring it out) 
Make a copy of the file zookeeper.properties and update the port. You must set up one port for each broker and a different `dataDir` for them. Then in separate terminals run each zookeeper instance with the following command (pay attention to the name):

````
bin/zookeeper-server-start.sh config/zookeeper_0.properties
````

````
bin/zookeeper-server-start.sh config/zookeeper_1.properties
````

````
bin/zookeeper-server-start.sh config/zookeeper_2.properties
````

**2. create the configuration files for each one of the three brokers**
````
cp server.properties server_0.properties
cp server.properties server_1.properties
cp server.properties server_2.properties
````

In the config files change the following lines according to the broker: 

- Set a unique id for each broker by changing the line 24 `broker.id=0`.
- Set a unique port by changing the line 34 `listeners=PLAINTEXT://:9092`
- Set a unique name for the log files by changing the line 62 `log.dirs=/tmp/kafka_0-logs`.
- Set a unique port for the connection by changing the line 125 `zookeeper.connect=localhost:2181`. Consider the ports set in the previews step. 

**3. Initiate the brokers**

In different terminals run the following command for each one of the config files created in the previews step (pay attention to the name).
````
bin/kafka-server-start.sh config/server_0.properties
````
````
bin/kafka-server-start.sh config/server_1.properties
````
````
bin/kafka-server-start.sh config/server_2.properties
````
**4. Create a topic**
Run the following command:
````
bin/kafka-topics.sh 
--create --topic replicated_topic 
--bootstrap-server localhost:9092 
--replication-factor 3 
--partitions 1
````
