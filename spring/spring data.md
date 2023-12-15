## mongo
### 普通模式
```yaml
version: '3.1'
spring:
  data:
    mongodb:
      host:
      database:
      port:
```
### url模式(开启认证)
```yaml
version: '3.1'
spring:
  data:
    mongodb:
      uri: mongodb://user:pw@host1/database
```

### url模式(replicate Set)
```yaml
version: '3.1'
spring:
  data:
    mongodb:
      uri: mongodb://host1,host2,host3/database?connect=replicaSet&slaveOK=true&replicaSet=rs0
```

### url模式(replicate Set)-开启认证
```yaml
version: '3.1'
spring:
  data:
    mongodb:
      uri: mongodb://user:pw@host1,host2,host3/database?connect=replicaSet&slaveOK=true&replicaSet=rs0
```
这里的host1,host2,host3是指replica set的三个节点，database是指数据库名，replicaSet=rs0是指replica set的名字，`slaveOK=true`是指允许从节点读取数据。