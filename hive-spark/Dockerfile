FROM kietly/centos7-java8
MAINTAINER Kiet Ly <kiet.ly@finra.org>


ENV HADOOP_PREFIX /Cloudera/hadoop 
ENV HADOOP_CONF_DIR=$HADOOP_PREFIX/etc/hadoop HADOOP_COMMON_HOME=$HADOOP_PREFIX HADOOP_HDFS_HOME=$HADOOP_PREFIX HADOOP_MAPRED_HOME=$HADOOP_PREFIX HADOOP_YARN_HOME=$HADOOP_PREFIX 
ENV HIVE_HOME /Cloudera/hive 
ENV HIVE_CONF_DIR $HIVE_HOME/conf 
ENV SPARK_HOME /Cloudera/spark 
ENV SPARK_CONF_DIR $SPARK_HOME/conf 
ENV DERBY_HOME=/Cloudera/derby/
ENV DERBY_INSTALL=$DERBY_HOME
ENV PATH $HADOOP_PREFIX/bin:$SPARK_HOME/bin:$HIVE_HOME/bin:$DERBY_HOME/bin:$PATH

RUN mkdir -p /Cloudera/warehouse ; mkdir -p /tmp/spark/log
WORKDIR /Cloudera

# Download Cloudera tarballs & derby
RUN wget --no-cookies --no-check-certificate "http://archive.cloudera.com/cdh5/cdh/5/hadoop-latest.tar.gz" ; wget --no-cookies --no-check-certificate "http://archive.cloudera.com/cdh5/cdh/5/hive-latest.tar.gz" ; wget --no-cookies --no-check-certificate "http://archive.cloudera.com/cdh5/cdh/5/spark-latest.tar.gz"; wget --no-cookies --no-check-certificate "http://shinyfeather.com//db/derby/db-derby-10.12.1.1/db-derby-10.12.1.1-bin.tar.gz"
# use this for speeding up development
#COPY hadoop-latest.tar.gz /Cloudera  
#COPY hive-latest.tar.gz /Cloudera 
#COPY spark-latest.tar.gz /Cloudera 
#COPY db-derby-10.12.1.1-bin.tar.gz /Cloudera

# unzip and rename directories
RUN tar xvfz hadoop-latest.tar.gz ; tar xvfz hive-latest.tar.gz ; tar xvfz spark-latest.tar.gz; tar xvfz db-derby-10.12.1.1-bin.tar.gz ; rm *.tar.gz ; mv hadoop-*/ hadoop/  ; mv hive-*/ hive/ ; mv spark-*/ spark/; mv db-derby-*/ derby/

# Update hadoop,hive and spark conf

COPY conf/core-site.xml $HADOOP_CONF_DIR
COPY conf/hive-env.sh   $HIVE_CONF_DIR
COPY conf/hive-site.* $HIVE_CONF_DIR/
COPY conf/hive-site.* $SPARK_CONF_DIR/
COPY conf/spark-class   $SPARK_HOME/bin
COPY conf/log4j.properties $SPARK_CONF_DIR
COPY conf/spark-defaults.conf $SPARK_CONF_DIR
COPY conf/spark-env.sh $SPARK_CONF_DIR
COPY derbyclient.jar $HIVE_HOME/lib
COPY derbytools.jar $HIVE_HOME/lib

# install missing command, snappy for spark
RUN  yum -y install snappy snappy-devel which; yum clean all

# update boot script
COPY bootstrap.sh /etc/bootstrap.sh
RUN chmod +x $SPARK_HOME/bin/spark-class ; chown root.root /etc/bootstrap.sh ; chmod 700 /etc/bootstrap.sh

EXPOSE 9083 1527 

ENTRYPOINT ["/etc/bootstrap.sh"]