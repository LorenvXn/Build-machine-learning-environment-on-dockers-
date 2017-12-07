
<b> Creating Spark container </b>

 <i> with indications from link: https://github.com/LorenvXn/Spark-Cassandra-Zeppelin-Integration-on-Docker </i>



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
<b> Creating Zeppelin container</b>

 <i> with indication from linkhttps://github.com/LorenvXn/Zeppelin-on-Docker---Perl-script </i>
 
  - For Zeppelin container creation run script <i>zepp.pl</i>
  




 <b>Creating Cassandra container</b>



- For Cassandra container, we will be using the official repository: </br> 
```
root@server# docker run --name=cassandra -d cassandra
```
