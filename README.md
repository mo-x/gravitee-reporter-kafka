[![Build Status](https://ci.gravitee.io/buildStatus/icon?job=gravitee-io/gravitee-reporter-kafka)](https://ci.gravitee.io/job/gravitee-io/job/gravitee-reporter-kafka/job/master/)

# gravitee-reporter-kafka

Report GraviteeIO Gateway request events to Kafka brokers

## Build

This plugin require :

* Maven 3
* JDK 8

Once built, a plugin archive file is generated in : target/gravitee-reporter-kafka-1.0.0-SNAPSHOT.zip

## Deploy

Just unzip the plugin archive in your gravitee plugin workspace ( default is : ${node.home}/plugins )

## Configuration

The configuration is loaded from the common GraviteeIO Gateway configuration file (gravitee.yml)
All kafka producer properties are allowed and available on
the [official documentation website](https://kafka.apache.org/documentation/#producerconfigs) .

Please note compatibility of this plugin with yours Kafka Brokers. Vertx kafka client 3.5.0 uses Kafka 0.10.2.1 , 3.5.1
uses Kafka 1.0.0. Currently, this plugin relies on 3.5.0. See:

* https://cwiki.apache.org/confluence/display/KAFKA/Compatibility+Matrix
* https://spring.io/projects/spring-kafka (for embedded tests)

Currently :

Example :

For a secured Kafka with SSL and Kerberos。
If you want to use this plug-in, you need to modify gravitee.yml
like this
```YAML
reporters:
  kafka:
    topic: gateway_log_topic
    hosts:
      - node1:6062
      - node2:6062
    type:
      - log
      - monitor
    java:
      security:
        krb5:
          conf: /opt/krb5.conf
    settings:
      acks: 1
      security:
        protocol: SASL_SSL
      sasl:
        jaas:
          config: >-
            com.sun.security.auth.module.Krb5LoginModule required
            useKeyTab=true
            refreshKrb5Config=true
            storeKey=true
            serviceName="kafka"
            keyTab="/opt/key.keytab"
            principal="foo@DOMAIN.COM";
```
