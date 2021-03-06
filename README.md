# kafka-java-mariadb-redis

<details><summary>windows</summary>
<p>
    
# Kafka download for Windows 
## 1. Create a Kafka directory.

    mkdir kafka 
    cd kafka

[Kafka Download page ](https://kafka.apache.org/downloads)
> In this project using Kafka Scala 2.12 Binary downloads:

Scala 2.12  - [kafka_2.12-2.7.0.tgz](https://apache.mirror.digitalpacific.com.au/kafka/2.7.0/kafka_2.12-2.7.0.tgz) 

## 2. Download the binary file into **kafka** folder and extract the file.
    cd kafka_2.12-2.7.0
## 3.Make configuration edits for Kafka
### Go to 
    Kafka\kafka_2.12-2.7.0\config
### Use any editer to edit the file

    server.properties 
### Modify the only below for Socket Server Settings and Log Basics
    listeners=PLAINTEXT://localhost:9092
    log.dirs=C:/Kafka/kafka_2.12-2.7.0/kafka-logs
### save and close
## 4.Start the zookeeper service 
    .\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
![text](https://github.com/sada498/kafka-java-mariadb-redis/blob/main/img/zookeeper.JPG)
> Open new windows powerShell in c:\ kafka\kafka_2.12-2.7.0
## 5.Start the kafka service
    .\bin\windows\kafka-server-start.bat .\config\server.properties
![text](https://github.com/sada498/kafka-java-mariadb-redis/blob/main/img/kafka.JPG)    
## 6. To check all topics are created in kafka
    .\bin\windows\kafka-topics.bat --list --zookeeper localhost:2181   
 </p>

</details> 
<details><summary>Ubuntu or Mac</summary>
<p>
    
# kafka download for Ubuntu or Mac 
    mkdir kafka
    cd kafka
## 1. Download the Kafka 
    wget https://apache.mirror.digitalpacific.com.au/kafka/2.7.0/kafka_2.12-2.7.0.tgz

    tar -xvf kafka_2.12-2.7.0.tgz
### Change directory to
    cd kafka_2.12-2.7.0
## 2. Make configuration edits for Kafka
    vi config/server.properties
### Modify the only below for Socket Server Settings

    listeners=PLAINTEXT://localhost:9092
### create Log folder
    mkdir logs
## 3. Start the zookeeper service
    ./bin/zookeeper-server-start.sh ./config/zookeeper.properties > ./logs/start_zk.log &
## 4.Start the kafka service
    ./bin/kafka-server-start.sh ./config/server.properties > ./logs/start_kafka.log &
## 5. Check the kafka running
    ps -ef|grep kafka
</p>

</details>

# create topic 
>windows

    cd Kafka\kafka_2.12-2.7.0\
    .\bin\windows\kafka-topics.bat  --zookeeper localhost:2181  --create  --topic streaming.orders.input  --partitions 1   --replication-factor 1

>other mac or Linux

    ./bin/kafka-topics.sh \
        --zookeeper localhost:2181 \
        --create \
        --topic streaming.orders.input \
        --partitions 1 \
        --replication-factor 1

# Docker install and config
> Install a docker on your local machine 
[Docker install](https://docs.docker.com/get-docker/)
## 1. Docker mariadb setup
    docker run  --name streaming-mariadb `
                 -v c:/mariadb:/var/lib/mysql `
                 -e MYSQL_ROOT_PASSWORD=streaming `
                 -e MYSQL_USER=streaming `
                 -e MYSQL_PASSWORD=streaming `
                 -p 3306:3306 `
                 -d mariadb `
## 2. Login into the mysql client ###

    docker exec -it streaming-mariadb mysql -u root -pstreaming

## 3. Execute the following commands ###

    CREATE DATABASE streaming;
    
    USE streaming;
    
    grant all on streaming to 'streaming'@'%';
    
    CREATE TABLE `order_summary` (
      `ID` int(11) NOT NULL AUTO_INCREMENT,
      `INTERVAL_TIMESTAMP` varchar(45) DEFAULT NULL,
      `PRODUCT` varchar(45) DEFAULT NULL,
      `TOTAL_VALUE` double DEFAULT NULL,
      PRIMARY KEY (`ID`)
    ) ENGINE=InnoDB DEFAULT CHARSET=latin1;
    
    grant all on streaming.order_summary to 'streaming'@'%';
    
> before you run the application make sure below services running on your local machine or server
1. Zookeeper
2. Kafka
3. Mariadb on docker containers

run StreamingAnalytics class.

**output**
![](https://github.com/sada498/kafka-java-mariadb-redis/blob/main/img/output.JPG)

Done! :)


