---
layout: post
title: How to Deploy a Kafka / Zookeeper cluster in Google Cloud Platform
---
![Apache Kafka]({{ site.baseurl}}/images/apache-kafka.png)

One of the great advantages of Google Cloud Platform is how easy and fast it is to run experiments. For example, you can easily spin up a Zookeper and Kafka cluster in a matter of minutes with very little configuration.

Note: The clusters below are not suitable for massive production use but good enough to run some tests

### Zookeeper

Lets spin up 3 machines that will form our Zookeeper cluster. The command below can be run from the [Google Cloud Shell](https://cloud.google.com/shell/docs/quickstart) or from your PC using the [GCP CLI](https://cloud.google.com/sdk/). All that you need to substitute are the project and instance name (e.g. zook-1)

We will name the Zookeeper VM instances: zook-1, zook-2, and zook-3

```bash
gcloud compute --project "[YOUR-PROJECT]" instances create "[INSTANCE-NAME]" --zone "europe-west1-c" --machine-type "n1-standard-1" --subnet "default" --maintenance-policy "MIGRATE" --service-account "1077112676311-compute@developer.gserviceaccount.com" --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring.write","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --tags "http-server" --image "centos-7-v20170523" --image-project "centos-cloud" --boot-disk-size "10" --boot-disk-type "pd-standard" --boot-disk-device-name "[INSTANCE-NAME]"
```

Once the VM is running, we can ssh to it and run the following command to install some dependencies and download Kafka.

```bash
sudo su
yum install -y wget nano java
mkdir /tmp/zookeeper
touch /tmp/zookeeper/myid
echo "ZOOKEEPER-ID" > zookeeper/myid
wget http://mirror.ox.ac.uk/sites/rsync.apache.org/kafka/0.10.2.1/kafka_2.12-0.10.2.1.tgz
tar -xvzf kafka_2.12-0.10.2.1.tgz
cd kafka_2.12-0.10.2.1
```

Modify the zookeeper properties to include all the instance details.

```bash
nano config/zookeeper.properties
```

Set the property values below:

```properties
tickTime=2000
initLimit=10
syncLimit=5
maxClientCnxns=30
server.1=zook-1:2888:3888
server.2=zook-2:2888:3888
server.3=zook-3:2888:3888
```

Once we've update the settings of all Zookeeper server properties, we can start them

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

### Kafka

Lets spin up 3 machines that will form our Kafka cluster. All that you need to substitute are the project and instance name (e.g. kafka-1)

gcloud compute --project "[YOUR-PROJECT]" instances create "[INSTANCE-NAME]" --zone "europe-west1-c" --machine-type "n1-standard-1" --subnet "default" --maintenance-policy "MIGRATE" --service-account "1077112676311-compute@developer.gserviceaccount.com" --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring.write","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --tags "http-server" --image "centos-7-v20170523" --image-project "centos-cloud" --boot-disk-size "10" --boot-disk-type "pd-standard" --boot-disk-device-name "[INSTANCE-NAME]"

