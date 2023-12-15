## docker mongodb 创建副本集 replica set
1. 创建网络
    ```shell
    docker network create mongoCluster
    ```
2. 创建容器
    ```shell
   docker run -d --rm -p 27017:27017 --name mongo1 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --bind_ip localhost,mongo1
    docker run -d --rm -p 27018:27017 --name mongo2 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --bind_ip localhost,mongo2
    docker run -d --rm -p 27019:27017 --name mongo3 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --bind_ip localhost,mongo3
    ```
3. 初始化副本集
    ```shell
    docker exec -it mongo1 mongo \
    --eval "rs.initiate(
      {
        _id : "myReplicaSet",
        members: [
          { _id : 0, host : "mongo1:27017" },
          { _id : 1, host : "mongo2:27017" },
          { _id : 2, host : "mongo3:27017" }
        ]
      }
    )"
    ```
4. 查看副本集状态
    ```shell
    docker exec -it mongo1 mongo --eval "rs.status()"
    ```
   
## docker-compose

```yaml
version: '3.1'
services:
   mongo1:
      image: mongo:5
      container_name: mongo1
      restart: always
      ports:
         - "27017:27017"
      networks:
         - mongoCluster
      command: mongod --replSet myReplicaSet --bind_ip localhost,mongo1
   mongo2:
      image: mongo:5
      container_name: mongo2
      restart: always
      ports:
         - "27018:27017"
      networks:
         - mongoCluster
      command: mongod --replSet myReplicaSet --bind_ip localhost,mongo2
   mongo3:
      image: mongo:5
      container_name: mongo3
      restart: always
      ports:
         - "27019:27017"
      networks:
         - mongoCluster
      command: mongod --replSet myReplicaSet --bind_ip localhost,mongo3
```