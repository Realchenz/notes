## mongodb docker
```shell
docker run -p 27017:27017 --name mongo \
-v /Users/chenzhenxu/DockerData/mongo/db:/data/db \
-d mongo:4

docker run -p 27017:27017 --name mongo \
-v /Users/chenzhenxu/DockerData/mongo/db01:/data/db \
-d mongo:4
```

## redis docker
```shell
docker run -p 80:80 --name nginx \
-v /Users/chenzhenxu/DockerData/nginx/html:/usr/share/nginx/html \
-v /Users/chenzhenxu/DockerData/nginx/logs:/var/log/nginx  \
-d nginx:1.22

docker run -p 80:80 --name nginx \
-v /Users/chenzhenxu/DockerData/nginx/html:/usr/share/nginx/html \
-v /Users/chenzhenxu/DockerData/nginx/logs:/var/log/nginx  \
-v /Users/chenzhenxu/DockerData/nginx/conf:/etc/nginx \
-d nginx:1.22
```
## mall-admin docker
```shell
docker run -p 8080:8080 --name mall-admin \
--link nacos-registry:nacos-registry \
--link mysql:db \
--link redis:redis \
-v /Users/chenzhenxu/DockerData/etc/localtime:/etc/localtime \
-v/Users/chenzhenxu/DockerData/app/mall-admin/logs:/var/logs \
-d mall/mall-admin:1.0-SNAPSHOT
```

## minIO docker
```shell
docker run -p 9090:9000 -p 9001:9001 --name minio \
-v /Users/chenzhenxu/DockerData/minio/data:/data \
-e MINIO_ROOT_USER=minioadmin \
-e MINIO_ROOT_PASSWORD=minioadmin \
-d minio/minio server /data --console-address ":9001"
```

## kibana docker
```shell
docker run --name kibana -p 5601:5601 \
-e "elasticsearch.hosts=http://es:9200" \
-d kibana:7.17.3
```
## es docker
```shell
docker run -p 9200:9200 -p 9300:9300 --name elasticsearch \
—net somenetwork
-e "discovery.type=single-node" \
-e "cluster.name=elasticsearch" \
-e "ES_JAVA_OPTS=-Xms512m -Xmx1024m" \
-v /Users/chenzhenxu/DockerData/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-v /Users/chenzhenxu/DockerData/elasticsearch/data:/usr/share/elasticsearch/data \
-d elasticsearch:8.11.0
```

## logstash docker
```shell
docker run --name logstash -p 4560:4560 -p 4561:4561 -p 4562:4562 -p 4563:4563 \
--link elasticsearch:es \
-v /Users/chenzhenxu/DockerData/logstash/logstash.conf:/usr/share/logstash/pipeline/logstash.conf \
-d logstash:7.17.3
```

## rabbitmq docker
```shell
docker run -p 5672:5672 -p 15672:15672 --name rabbitmq \
-v /Users/chenzhenxu/DockerData/rabbitmq/data:/var/lib/rabbitmq \
-d rabbitmq:3.9.11-management
```
## redis docker
```shell
docker run -p 6379:6379 --name redis \
-v /Users/chenzhenxu/DockerData/redis/data:/data \
-d redis:7 redis-server --appendonly yes
```
## mysql docker
```shell
docker run -p 3306:3306 --name mysql \
-v /Users/chenzhenxu/DockerData/mysql/log:/var/log/mysql \
-v /Users/chenzhenxu/DockerData/mysql/data:/var/lib/mysql \
-v /Users/chenzhenxu/DockerData/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root  \
-d mysql:5.7
```