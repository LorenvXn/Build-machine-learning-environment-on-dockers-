

Once you've started the Spark with the proper dependencies, from spark-shell, use the following scala lines:


```
import org.apache.spark._
import _root_.kafka.serializer.StringDecoder
import org.apache.spark.SparkContext._
import org.apache.spark.streaming._

import org.apache.spark.streaming.StreamingContext._
import org.apache.spark.streaming.kafka._

import com.datastax.spark.connector._
import com.datastax.spark.connector.streaming._
import com.datastax.spark.connector.cql.CassandraConnector

//set-up Kafka env
val brokers = "172.17.0.6:9092"
val topics = "streaming-topic"
val topicsSet = topics.split(",").toSet

//setup Cassandra IP host...in this scenario, it's 172.17.0.3

val conf = new SparkConf(true).set("spark.cassandra.connection.host", "172.17.0.3")

val sc=new SparkContext(conf)
val ssc = new StreamingContext(sc, Seconds(10))

val kafkaParams = Map[String, String]("metadata.broker.list" -> brokers)

val rawDstream = KafkaUtils.createDirectStream[String, String, StringDecoder, StringDecoder](ssc, kafkaParams, topicsSet)

rawDstream.count().map(cnt =>" ############# received events: " + cnt).print()

 
val hdfsdir = " /tmp/3v1l_deedz111/"

//create Cassandra keyspace & table if none

CassandraConnector(conf).withSessionDo { session => session.execute("CREATE KEYSPACE IF NOT EXISTS streaming_key WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1 }")}

CassandraConnector(conf).withSessionDo { session => session.execute("CREATE TABLE IF NOT EXISTS streaming_key.streaming_cass (DATE TEXT PRIMARY KEY, LON  VARCHAR, LAT VARCHAR, BASE VARCHAR)")}


//store streams to Cassandra

 val lines = rawDstream.map(line => line._2.split(',')).map(s => (s(0).toDouble, s(1).toString,s(2).toDouble, s(3).toDouble))
 
 lines.saveToCassandra("streaming_key ", "streaming_cass", SomeColumns("date", "lon", "lat", "base"))
 
//store streams under hdfs dir

lines.foreachRDD(rdd => {if (rdd.count() > 0) { rdd.saveAsTextFile(hdfsdir) } })

//start le wild streaming

ssc.start()
```
