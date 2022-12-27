# Kafka

## Setup Cluster

### Java

```shell
sudo apt install openjdk-11-jre-headless
```

### Install Zookeeper

```shell
wget https://dlcdn.apache.org/zookeeper/zookeeper-3.8.0/apache-zookeeper-3.8.0-bin.tar.gz
tar -xvf apache-zookeeper-3.8.0-bin.tar.gz
sudo mkdir /var/zookeeper
sudo chown ubuntu:ubuntu /var/zookeeper/
vi apache-zookeeper-3.8.0-bin/conf/zoo_sample.cfg
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/var/zookeeper
clientPort=2181
server.0=ip-10-0-10-52.eu-west-1.compute.internal:2888:3888
server.1=ip-10-0-0-167.eu-west-1.compute.internal:2888:3888

mv apache-zookeeper-3.8.0-bin/conf/zoo_sample.cfg apache-zookeeper-3.8.0-bin/conf/zoo.cfg
vi /var/zookeeper/myid
0


```
