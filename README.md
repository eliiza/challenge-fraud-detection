# Eliiza Fraud Detection Challenge

## Intro

In this challenge you're given data containing some credit card transactions.

## Challenge

Because the data is streaming through a Kafka topic, the challenge consists in tapping onto that topic, consuming the
credit card transactions and identifying **the credit card numbers** subject to potential *fraud when the sum of
expenses within any span of 24 hours exceeds $100.00*.

## How to solve the challenge

For this challenge you can use any language you like.  You will most probably use a Scala, Python, Java or JavaScript
(Node.js) Kafka client; we have no preference at all.  We provide a `docker-compose.yml` file containing images with a
Kafka environment (1 ZooKeeper, 1 Kafka broker and a Schema Registry) like this:
- bootstrap server: `localhost:9092`
- credit card transactions topic name: `au.com.eliiza.cctransactions.avro`
- Schema Registry URL: `http://localhost:8081`

Note that the topic contains data in the **[AVRO](https://avro.apache.org/docs/current/spec.html)** format, serialized
with Confluent's
[Wire Format](https://docs.confluent.io/platform/current/schema-registry/serdes-develop/index.html#wire-format).  To
inspect the schema for the credit card transactions topic, head to the following Schema Registry endpoint:
[http://localhost:8081/subjects/au.com.eliiza.cctransactions.avro-value/versions/latest/schema](http://localhost:8081/subjects/au.com.eliiza.cctransactions.avro-value/versions/latest/schema).

**Tips**:
- By using a deserialiser from Confluent (there are some available
[here](https://github.com/confluentinc/schema-registry/tree/master/avro-serde/src/main/java/io/confluent/kafka/streams/serdes/avro),
you don't need to bother with wire format's details, and can read an event value straight into a
[GenericRecord](https://github.com/apache/avro/blob/master/lang/java/avro/src/main/java/org/apache/avro/generic/GenericRecord.java)
or a
[SpecificRecord](https://github.com/apache/avro/blob/master/lang/java/avro/src/main/java/org/apache/avro/specific/SpecificRecord.java).
- The [Kafka Streams](https://kafka.apache.org/documentation/streams/) library offers
[Sliding Windows](https://kafka.apache.org/31/documentation/streams/developer-guide/dsl-api.html#sliding-time-windows)
to help process the above topic, if you fancy that library.

## Starting the Kafka environment with Docker Compose

First of all, install [Docker Compose](https://docs.docker.com/compose/install/) for your system, if you don't already
have it.  Then:

    $ git clone https://github.com/eliiza/challenge-fraud-detection.git myrepo
    $ cd myrepo
    $ docker-compose up

After all the images are downloaded and Kafka is up and running, you should start seeing our service logging the
production of credit card transactions (and a few other events into other topics).  Once you solve the challenge, send
us back your code and the set of credit cards presenting potential fraud.

Good luck!!!
