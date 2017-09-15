
Perform the checkings: 

<b> From HDFS side </b>

```
bash-4.1# hadoop fs -ls /tmp/3v1l_deedz111
Found 2 items
-rw-r--r--   1 root supergroup          0 2017-09-14 06:13 /tmp/3v1l_deedz111/_SUCCESS
-rw-r--r--   1 root supergroup       2564 2017-09-14 06:13 /tmp/3v1l_deedz111/part-00000
bash-4.1# hadoop fs -cat /tmp/3v1l_deedz111/part-00000
(2014-08-01 00:00:00,40.729,-73.9422,B02598)
(2014-08-01 00:00:00,40.7476,-73.9871,B02598)
(2014-08-01 00:00:00,40.7424,-74.0044,B02598)
[---------------snip----------------------------]

```


<b>From Cassandra's side</b>

```
 cqlsh>use streaming_key;
 cqlsh:streaming_key>select * from streaming_cass;

 date                | base   | lat      | lon
---------------------+--------+----------+---------
 2014-08-01 00:00:00 | B02598 | -73.9422 | 40.729
 2014-08-01 00:00:00 | B02598 | -73.987  | 40.7476
 2014-08-01 00:00:00 | B02764 | -74.0347 | 40.7424
 
 [----------------snip-----------------------------]
 ```
 
 
<b> Well, well... looks like it's time for Zeppelin to appear: [BuildEnvironment_part3](https://github.com/Satanette/Build-machine-learning-environment-on-dockers-/blob/master/BuildEnvironment_part3.md) </b>
 
 <i> For indications on how to use this Zeppelin container, go to: [Zeppelin on Docker] (https://github.com/Satanette/Zeppelin-on-Docker---Perl-script)
 ...it contains a script which finds Zeppelin's IP & pops it out in a browser. </i>
 
 
 <i> the very small csv file can be found here: 
 [data](https://github.com/Satanette/Build-machine-learning-environment-on-dockers-/tree/master/data)
 (yuh, yuh... too lazy; I used a piece of a csv already on the Internetz, from uber:
  https://raw.githubusercontent.com/caroljmcdonald/mapr-spark-streaming-ml-uber/master/data/uber.csv </i>
