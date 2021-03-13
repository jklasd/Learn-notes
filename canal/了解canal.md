# Canal核心功能介绍

### 一、binlog读取

***

1、CanalMQStarter

通信队列的服务端线程池，为队列发送者，发送信息。

2、CanalServerWithEmbedded

读取binlog



CanalMQStarter在线程中循环调用CanalServerWithEmbedded#getWithoutAck，获取最新信息。

3、CanalInstanceWithManager/CanalInstanceWithSpring



4、MemoryEventStoreWithBuffer



5、MysqlConnection#sendBinlogDump

