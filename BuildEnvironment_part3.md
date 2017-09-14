
<b> Time to go to Zeppelin </b>

From a Zeppelin note, try your new environment:

```
import org.apache.spark._
import org.apache.spark.streaming._
import org.apache.commons.io.IOUtils
import java.net.URL
import java.nio.charset.Charset
import org.apache.spark.sql.SQLContext
import org.apache.spark.sql.types._
import org.apache.spark.sql.functions._
import org.apache.spark.sql._
import org.apache.spark.ml.feature.{IndexToString, StringIndexer, VectorIndexer}

import org.apache.spark.sql.SQLContext
import org.apache.spark.sql.functions._
import java.text.SimpleDateFormat
import java.util.Calendar
import sqlContext.implicits._
import org.apache.spark.sql.Row


 val schema = StructType(Array(
      StructField("date", StringType, true),
      StructField("lon", DoubleType, true),
      StructField("lat", DoubleType, true),
      StructField("base", StringType, true)  ))


```

Access data stored in HDFS file

```
val coord_file = sc.textFile("hdfs://172.17.0.5:9000/tmp/3v1l_deedz111/part-00000")


val coord_temp = coord_file.map(_.split(",")).map(c => Row( c(0).toString ,c(1).toDouble ,c(2).toDouble,c(3).toString))
val coord_df = sqlContext.createDataFrame(coord_temp, schema)

//  coord_df.printSchema()
```

Do some checkings!

```
coord_df.select("lat", "lon", "date").show()

+--------+-------+--------------------+
|     lat|    lon|                date|
+--------+-------+--------------------+
|-73.9422| 40.729|2014-08-01 00:00:00|
|-73.9871|40.7476|2014-08-01 00:00:00|
|-74.0044|40.7424|2014-08-01 00:00:00|
|-73.9869| 40.751|2014-08-01 00:00:00|
|-73.9902|40.7406|2014-08-01 00:00:00|
|-73.9591|40.6994|2014-08-01 00:00:00|
|-73.9398|40.6917|2014-08-01 00:00:00|
|-73.9223|40.7063|2014-08-01 00:00:00|
|-74.0168|40.6759|2014-08-01 00:00:00|
|-73.9847|40.7617|2014-08-01 00:00:00|
|-73.9064|40.6969|2014-08-01 00:00:00|
|-73.9751|40.7623|2014-08-01 00:00:00|
|-73.9669|40.6982|2014-08-01 00:00:00|
|-73.9253|40.7553|2014-08-01 00:00:00|
|-73.9876|40.7325|2014-08-01 00:00:00|
| -74.017|40.6754|2014-08-01 00:00:00|

```
Create a temporary Spark SQL table 

```
coord_df.registerTempTable("coordz")

```


Do some checking with a simple SQL query (and from here on, you can apply whatevs predictions you like!


![ScreenShot](https://github.com/Satanette/test/blob/master/do_a_flip.png)
<i> (waht?! I only said I'd build the env, not prepare a tutorial on machine learning algorithms... you're on your own from here on!) </i>

As a bonus, you can use Zeppelin with Cassandra, just beware of the dependencies! 
