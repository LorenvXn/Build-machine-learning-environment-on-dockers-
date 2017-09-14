# Build-machine-learning-environment-on-dockers-

<b> [1] Build dockers pas per indications from every folder - Kafka, Spark..etc</b>

You should be getting a similar output, and the IDs you are noticing here will be used as exemplification:

```
root@tron:/opt# docker ps
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS                                                                                                                                                                       NAMES
ca4573e7b9f0        zeppelin_1               "/bin/bash"              7 days ago          Up 7 days           0.0.0.0:8080->8080/tcp                                                                                                                                                      pedantic_mcnulty
e2f52f3503ad        zookeeper                "/opt/zookeeper/bin/z"   7 days ago          Up 7 days           2181/tcp, 2888/tcp, 3888/tcp                                                                                                                                                jovial_hodgkin
517f21e4bbd5        kafka                    "/bin/bash"              7 days ago          Up 7 days                                                                                                                                                                                       sick_wilson
1dce098d5dd5        cassandra                "/docker-entrypoint.s"   7 days ago          Up 7 days           7000-7001/tcp, 7199/tcp, 9042/tcp, 9160/tcp                                                                                                                                 cassandra
b1040d984f62        sparky                   "/etc/bootstrap.sh ba"   7 days ago          Up 7 days           22/tcp, 8030-8033/tcp, 0.0.0.0:4040->4040/tcp, 0.0.0.0:8042->8042/tcp, 8040/tcp, 49707/tcp, 50010/tcp, 50020/tcp, 50070/tcp, 50075/tcp, 50090/tcp, 0.0.0.0:8088->8088/tcp   berserk_brahmagupta
```


<b> [2] Connect dockers </b>

<i> a.) Create spark_streaming_bridge </i>

root@tron:/opt/mwaha# docker network create --driver=bridge spark_streaming_bridge

<i> b.) Add containers to newly created bridge </i>
root@tron:/opt/mwaha# docker network connect spark_streaming_bridge 1dce098d5dd5
root@tron:/opt/mwaha# docker network connect spark_streaming_bridge 5c53e18e0023
root@tron:/opt/mwaha# docker network connect spark_streaming_bridge 026b999ffae3
root@tron:/opt/mwaha# docker network connect spark_streaming_bridge 3a17d4455a18
root@tron:/opt/mwaha# docker network connect spark_streaming_bridge b1040d984f62

<i> More indications for point 2. at: https://github.com/Satanette/Connecting-containers-with-docker-network </i> 

<b> [3.] Make the necessary changes for configuration files </b>


Modify file /kafka/config/server.properties from  kafka's container, by adding the IP of zookeeper's container

In this scenario, the zookeeper's IP is 172.17.0.7
```
[root@517f21e4bbd5 config]# more /kafka/config/server.properties | grep "zookeeper.connect="
zookeeper.connect=172.17.0.7:2181

```
<b> [4.]  Let's make the environment work  <b>

<i> a) Start Zookeeper service </i>

```
root@tron:/opt# docker exec -ti `docker ps | grep zookeeper | awk {'print $1'}` /bin/bash
root@e2f52f3503ad:/opt/zookeeper# cd bin/
root@e2f52f3503ad:/opt/zookeeper/bin# ./zkServer.sh start
JMX enabled by default
Using config: /opt/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
root@e2f52f3503ad:/opt/zookeeper/bin#
```

<i> b) Go to Kafka container and start broker and create new topic </i>
```
root@tron:/opt# docker exec -ti `docker ps | grep kafka | awk {'print $1'}` /bin/bash
[root@517f21e4bbd5 /]# cd kafka
[root@517f21e4bbd5 kafka]#
[root@517f21e4bbd5 kafka]#bin/kafka-server-start.sh config/server.properties
[root@517f21e4bbd5 kafka]#
[root@517f21e4bbd5 kafka]#bin/kafka-topics.sh --create --zookeeper 172.17.0.7:2181 --replication-factor 1 --partitions 1 --topic streaming-topic
[root@517f21e4bbd5 kafka]#bin/kafka-console-producer.sh --broker-list 172.17.0.6:9092 --topic streaming-topic

```

<i> c) It's time to bring up Spark with Cassandra, Kafka dependencies </i>
```
root@tron:~# docker exec -ti `docker ps | grep spark | awk {'print $1'}` /bin/bash
bash-4.1#
bash-4.1# spark-shell --packages org.apache.spark:spark-streaming-kafka_2.10:1.6.1,datastax:spark-cassandra-connector:1.6.0-s_2.10 --jars kafka_2.10-0.10.1.2.1.2.0-10.jar

[=======snip=======]

Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 1.6.0
      /_/

[=======snip=======]

scala>
scala>


```

Now, time to test your environment from spark-shell, and do streaming: https://github.com/Satanette/Build-machine-learning-environment-on-dockers-/blob/master/Spark_Shell.md


After you started the streaming, from kafka (after making sure the broker is running), send a csv (the Cassandra table has been specifically created for this csv, as you might have noticed in Scala code):

```
root@517f21e4bbd5 kafka]#  bin/kafka-console-producer.sh --broker-list 172.17.0.6:9092 --topic streaming-topic < /opt/woohoo.csv
[root@517f21e4bbd5 kafka]#

```

Them, perform checking: https://github.com/Satanette/Build-machine-learning-environment-on-dockers-/blob/master/BuildEnvironment_part2.md


