
 <i> with indications from link: https://github.com/Satanette/Spark-Cassandra-Zeppelin-Integration-on-Docker </i>



  - Run perl script <i>spark.pl</i> to create spark container.</br> 


You will be prompted to:

``bash-4.1#``

Start Spark Shell: 
```
bash-4.1#spark-shell \ 
--master yarn-client \  
--packages datastax:spark-cassandra-connector:1.6.0-s_2.10  

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
  - For Zeppelin container creation run script <i>zepp.pl</i>
  
  //indication from link https://github.com/Satanette/Zeppelin-on-Docker---Perl-script // 


  - Creating Cassandra container:



- For Cassandra container, we will be using the official repository: </br> 
```
root@server# docker run --name=cassandra -d cassandra
```
