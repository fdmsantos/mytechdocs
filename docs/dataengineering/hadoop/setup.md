# Setup HDFS Cluster (Ubuntu)

## All Nodes

### Setup Passwordless login Between Name Node and all Data Nodes.

```shell
#On Master Node
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat .ssh/id_rsa.pub >> ~/.ssh/authorized_keys
scp .ssh/authorized_keys datanode1:/home/ubuntu/.ssh/authorized_keys
scp .ssh/authorized_keys datanode2:/home/ubuntu/.ssh/authorized_keys
scp .ssh/authorized_keys datanode3:/home/ubuntu/.ssh/authorized_keys
```

### Install Java JDK

```bash
# Download from https://adoptopenjdk.net/
tar -xvf OpenJDK8U-jdk_x64_linux_hotspot_8u352b08.tar.gz
sudo mkdir -p /usr/lib/jvm
sudo mv ./jdk8u352-b08 /usr/lib/jvm/

# Create alternatives
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk8u352-b08/bin/java" 1
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk8u352-b08/bin/javac" 1
#sudo update-alternatives --install "/usr/bin/javaws" "javac" "/usr/lib/jvm/jdk8u352-b08/bin/javaws" 1

# Permissions
sudo chmod a+x /usr/bin/java 
sudo chmod a+x /usr/bin/javac 
#sudo chmod a+x /usr/bin/javaws
sudo chown -R root:root /usr/lib/jvm/jdk8u352-b08

# Set JAVA_HOME in /etc/environment
sudo vi /etc/environment
export JAVA_HOME=/usr/lib/jvm/jdk8u352-b08


# Alternative
sudo apt-get -y install openjdk-8-jdk-headless
```

### Install Hadoop

```bash
wget http://apache.cs.utah.edu/hadoop/common/hadoop-3.3.4/hadoop-3.3.4.tar.gz
tar -xzf hadoop-3.3.4.tar.gz 
mv hadoop-3.3.4 hadoop

# Apache Hadoop configuration
vi ~/.bashrc
export HADOOP_HOME=/home/ubuntu/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_MAPRED_HOME=${HADOOP_HOME}
export HADOOP_COMMON_HOME=${HADOOP_HOME}
export HADOOP_HDFS_HOME=${HADOOP_HOME}
export YARN_HOME=${HADOOP_HOME}

source ~/.bashrc
```

### Update hadoop-env.sh

```bash
vi ~/hadoop/etc/hadoop/hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/jdk8u352-b08
```

### core-site.xml

**File:** ~/hadoop/etc/hadoop/core-site.xml

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://<namenode_ip>:9000</value>
    </property>
</configuration>
```

### hdfs-site.xml

**File:** ~/hadoop/etc/hadoop/hdfs-site.xml

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>3</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:///usr/local/hadoop/hdfs/data</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:///usr/local/hadoop/hdfs/data</value>
    </property>
</configuration>
```

### yarn-site.xml

**File:** ~/hadoop/etc/hadoop/yarn-site.xml

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>ip-10-0-11-232.ec2.internal</value>
    </property>
    <property>
        <name>yarn.resourcemanager.resource-tracker.address</name>
        <value>ip-10-0-11-232.eu-west-1.compute.internal:8031</value>
    </property>
    <property>
        <name>yarn.resourcemanager.address</name>
        <value>ip-10-0-11-232.eu-west-1.compute.internal:8032</value>
    </property>
    <property>
        <name>yarn.resourcemanager.scheduler.address</name>
        <value>ip-10-0-11-232.eu-west-1.compute.internal:8030</value>
    </property>
    <property>
        <name>yarn.resourcemanager.admin.address</name>
        <value>ip-10-0-11-232.eu-west-1.compute.internal:8033</value>
    </property>
    <property>
        <name>yarn.resourcemanager.webapp.address</name>
        <value>63.32.173.81:8088</value>
    </property>
    <property>
        <description>Classpath for typical applications.</description>
        <name>yarn.application.classpath</name>
        <value>
            $HADOOP_CONF_DIR,
            $HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,
            $HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,
            $HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,
            $HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/*
        </value>
    </property>
    <property>
        <name>yarn.log.aggregation.enable</name>
        <value>true</value>
    </property>
    <property>
        <description>Where to aggregate logs</description>
        <name>yarn.nodemanager.remote-app-log-dir</name>
        <value>hdfs://var/log/hadoop-yarn/apps</value>
    </property>
</configuration>
```

### Create Data Folder

```bash
sudo mkdir -p /usr/local/hadoop/hdfs/data
sudo chown ubuntu:ubuntu -R /usr/local/hadoop/hdfs/data
chmod 700 /usr/local/hadoop/hdfs/data
```

### Master and Workers Files

**File:** ~/hadoop/etc/hadoop/masters

```bash
ip-10-0-11-232.ec2.internal
```

**File:** ~/hadoop/etc/hadoop/workers

```bash
ip-10-0-148-183.ec2.internal
ip-10-0-159-158.ec2.internal
```

## NameNode

### yarn-site.xml

**File:** ~/hadoop/etc/hadoop/mapred-site.xml

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

## DataNodes

```sh
# Open Kernel Firewall port to upload HDFS
sudo iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 9866 -m comment --comment "Port 9866  datanode" -j ACCEPT
sudo iptables-save
```

## Start Cluster

```shell
hdfs namenode -format
start-dfs.sh
# To Check
jps
# Stop
stop-dfs.sh

# Yarn
start-yarn.sh
stop-yarn.sh

# Alternatives
start-all.sh
```


## Three essential things to do while building Hadoop environment

[blog](http://ahikmat.blogspot.com/2014/05/three-essential-things-to-do-while.html)