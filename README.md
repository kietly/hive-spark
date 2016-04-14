Apache Spark/Hive on Docker
==========

This docker integrate Hive and Spark in local mode. That mean Hive or Spark will run as a single JVM and write directly to local ext4 file system, no HDFS.


## Running the image from docker hub

* Docker must be installed in your Windows/Mac laptop. If you haven't, see this link https://docs.docker.com/windows/
* If using docker-machine on (mac/windows) make sure your (default) VM has more than 4GB memory. I recommended 8GB as minimum.
```
docker run -d --name spark kietly/hive-spark
```
and open a terminal
```
docker exec -it spark /bin/bash
```
* note if you see this error: "The name "/spark" is already in use by container" then restart existing container and open terminal
```
docker stop spark
docker start spark
docker exec -it spark /bin/bash
```

## Versions
    Hadoop 2.6.0, Hive 1.1.0 and Apache Spark v1.6.0 from Cloudera CDH 5.7 on Centos 7.2 

# Quick testing
* Run the spark shell
```
$root@a1s4s3> spark-shell 
```

* Execute the the following command which should return 1000
```
    scala> sc.parallelize(1 to 1000).count()
```

* Execute the the following command which should write the "Pi is roughly 3.1418" on screen
```
    $root@a1s4s3> run-example SparkPi 10
```
    
##### Test out the Spark with Hive tutorial here from Hortonwork, http://hortonworks.com/hadoop-tutorial/using-hive-with-orc-from-apache-spark/. Please note these different here since this is a local mode for development. 
* There is no HDFS, you can skip this 
```
    $hadoop fs -put ./yahoo_stocks.csv /tmp/
```
    
* No yarn client, simply run with no argument 
```
    $spark-shell
```
    
* open text file from local file system instead of HDFS assuming you put yahoo_stocks.csv in /tmp
```
    val yahoo_stocks = sc.textFile("file:///tmp/yahoo_stocks.csv")
```

* Everything else remain the same.
 