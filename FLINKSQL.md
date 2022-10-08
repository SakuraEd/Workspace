# flink-cpd-application

## 1.本地运行
clean install -Dmaven.test.skip=true -P dev

## 2.导入java IDE

## 3.本地开发
* 在IDE创建一个application,
* main函数：com.vivo.internet.ads.realtime.application.FlinkSqlApplication 
* 参数：--jobName mockdata --sql /文件路径/flink-cpd-application/flink-application/src/main/resources/recall/recall_cpd_billing.sql


## 4.线上打包

clean install -Dmaven.test.skip=true -P product


# FlinkSql开发指南
Flink-application简化了flink开发流程，降低使用flink处理数据的门槛，通过sql实现低代码处理实时数据流，任务流程分为source、transformer、sink三部分
目前sourc常用消费kafka和Pulsar源数据，transformer对数据加工统计，特征开发中常用sink支持数据写入kafka、redis、hive

## 1.DEMO
'''CREATE TABLE kafka_source(
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
'''sql
