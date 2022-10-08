-- {"serverTime":1623120270628,"creativeId":"20000002","bigintTest":12345678912345,"intTest":123,"doubleTest":2.333}
CREATE TABLE kafka_source(
    serverTime TIMESTAMP ,
    creativeId VARCHAR,
    bigintTest BIGINT,
    intTest INT,
    doubleTest DOUBLE
) WITH (
    'connector' = 'vivo-kafka',
    'connector.format' = 'kafka-json',
    'connector.topic' = 'flink_test',
    'connector.groupid' = 'flink_test',
    'connector.bootstrapserver' = '10.101.50.13:39092,10.101.50.14:39092,10.101.50.15:39092',
    'connector.kafka.auth.username' = 'ad_realtime_data',
    'connector.kafka.auth.password' = 'M3N2SFhZdm9lU0dw'
);

CREATE TABLE kafka_sink (
    serverTime TIMESTAMP ,
    creativeId VARCHAR,
    bigintTest BIGINT,
    intTest INT,
    doubleTest DOUBLE
) WITH (
    'connector' = 'vivo-kafka',
    'connector.format' = 'kafka-json',
    'connector.topic' = 'kafka_sink',
    'connector.bootstrapserver' = '10.101.50.13:39092,10.101.50.14:39092,10.101.50.15:39092',
    'connector.kafka.auth.username' = 'ad_realtime_data',
    'connector.kafka.auth.password' = 'M3N2SFhZdm9lU0dw'
);

insert into kafka_sink select * from kafka_source;

CREATE TABLE console_sink (
    serverTime TIMESTAMP ,
    creativeId VARCHAR,
    bigintTest BIGINT,
    intTest INT,
    doubleTest DOUBLE
) WITH (
    'connector' = 'vivo-console'
);

insert into console_sink select * from kafka_source;
