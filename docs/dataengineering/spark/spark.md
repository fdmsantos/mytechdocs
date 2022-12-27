# Setup Spark

## Resource Manager

```shell
wget https://dlcdn.apache.org/spark/spark-3.3.1/spark-3.3.1-bin-hadoop3.tgz
tar -xzf spark-3.3.1-bin-hadoop3.tgz
mv spark-3.3.1-bin-hadoop3 spark

vi ~/.bashrc

export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export SPARK_HOME=/home/ubuntu/spark
export PATH=$PATH:$SPARK_HOME/bin
export LD_LIBRARY_PATH=$HADOOP_HOME/lib/native:$LD_LIBRARY_PATH

source ~/.bashrc

vi $SPARK_HOME/conf/spark-defaults.conf
spark.master yarn
spark.driver.memory 512m
spark.yarn.am.memory 512m
spark.executor.memory 512m

# Test
spark-submit --deploy-mode client --class org.apache.spark.examples.SparkPi $SPARK_HOME/examples/jars/spark-examples_2.12-3.3.1.jar 10
```

## History Server

```shell
vi $SPARK_HOME/conf/spark-defaults.conf

spark.eventLog.enabled true
spark.eventLog.dir hdfs://namenode:9000/spark-logs
spark.history.provider org.apache.spark.deploy.history.FsHistoryProvider
spark.history.fs.logDirectory hdfs://namenode:9000/spark-logs
spark.history.fs.update.interval 10s
spark.history.ui.port 18080
spark.yarn.historyServer.address http://namenode:18088
spark.yarn.jars hdfs://ip-10-0-11-232.eu-west-1.compute.internal:9000/spark3-jars/*.jar

# Run
$SPARK_HOME/sbin/start-history-server.sh

# IS use spark user
hadoop fs -chown -R spark:spark /user/spark
hadoop fs -chmod 777 /user/spark/applicationHistory
# Edit $SPARK_HOME/conf/spark-defaults.conf
spark.eventLog.dir hdfs://quickstart.cloudera:8020/user/spark/applicationHistory
```

## Configure Spark with Hive running in postgres

```shell
wget https://repo1.maven.org/maven2/org/postgresql/postgresql/42.2.12/postgresql-42.2.12.jar
hadoop fs -mkdir /spark3-jars
hadoop fs -copyFromLocal postgresql-42.2.12.jar /spark3-jars/postgresql-42.2.12.jar # Needs to be dir configured in spark.yarn.jars
```