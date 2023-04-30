# apache_kafka

This repository is an exercise based on the DEMOS proposed in the course https://app.pluralsight.com/library/courses/apache-kafka-getting-started/table-of-contents

### Pre-requisites:


### Usage:

**1. start zookeeper with default configuration**

Run:
````
cd kafka_2.13-3.4.0
KAFKA_OPTS="-Dzookeeper.4lw.commands.whitelist=*" bin/zookeeper-server-start.sh
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
bin/kafka-topics.sh --create --topic my_topic --zookeeper localhost:2181 --replication-factor 1 --partitions 1
````
