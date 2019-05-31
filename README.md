# CSV reader, a Spring boot Kafka example

Goal of this app is to read a csv file and save it in a database. To prove kafka, we send the csv line to kafka and then read them and save them in the database.

## Apps

### Csv producer
Spring boot app which reads file `test.csv` and send a message per line to the kafka topic `test`.
Its stops when the whole file has been read
#### Install
```
cd csv-producer
mvn clean package
java -jar target/csv-producer-0.0.1-SNAPSHOT.jar
```

In logs, lines appear one by one

### csv-consumer-persist
Spring boot app which listen to the `test` topic and save received messages in a postgreSQL database.
Keep listening the topic, waiting for messages. Its idempotent, duplicated messages will not break it.
#### Install
There is 1 role and 2 database to add in postgres
```
psql -U postgres -c "CREATE ROLE kafka_example WITH ENCRYPTED PASSWORD '6iJ7RyW\!yc\!xmXW^' NOSUPERUSER CREATEDB INHERIT LOGIN"
psql -U postgres -c "CREATE DATABASE kafka_example WITH OWNER = kafka_example ENCODING 'UTF8'"
psql -U postgres -c "CREATE DATABASE kafka_example_test WITH OWNER = kafka_example ENCODING 'UTF8'"
```
flyway is used to manage the database schema

To compile and start:
```
cd csv-consumer-persist
mvn clean package
java -jar target/csv-consumer-persist-0.0.1-SNAPSHOT.jar
```

### csv-consumer-log
Spring boot app which listen to the `test` topic and write a log ligne for each received message.
#### Install
```
cd csv-consumer-log
mvn clean package
java -jar target/csv-consumer-log-0.0.1-SNAPSHOT.jar
```

### tools
In directory `dev-tools`, there is a `docker-compose.yml` file to start kafka. 2 json file to test the consumers. You can send them to kafka using kafkacat.
