```
# refer to 
https://debezium.io/documentation/reference/0.10/connectors/mysql.html
# Verify mysql server enabled bin logs

show variables like "log_bin";
show variables like "%binlog_format%";
show variables like "%binlog_row_image%";
show variables like "%expire_logs_days%";


# On mysql create user for REPLICATION

CREATE USER 'debezium'@'%' IDENTIFIED BY 'dbz';
GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'debezium'@'%';

# lab1.json

{
  "name": "lab1-connector",  
  "config": {
    "connector.class": "io.debezium.connector.mysql.MySqlConnector", 
    "database.hostname": "192.168.1.2", 
    "database.port": "3306", 
    "max.request.size": "4000000",
    "database.user": "debezium", 
    "database.password": "dbz", 
    "database.server.id": "1", 
    "database.server.name": "lab1", 
    "database.whitelist": "lab1", 
    "database.history.kafka.bootstrap.servers": "kafka1:9092", 
    "database.history.kafka.topic": "dbhistory.lab1", 
    "include.schema.changes": "true" 
  }
}

# Create connector
curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" localhost:8083/connectors -d@lab1.json

# verify

./bin/kafka-topics.sh --bootstrap-server 192.168.1.2:9092 --list
./bin/kafka-console-consumer.sh --bootstrap-server 192.168.1.2:9092 --topic lab1.lab1.hocsinh --from-beginning

```