# docker 指令
## 启动未启动的容器
`docker start $(docker ps -a | grep 'Exited' | awk '{print $1}')`
## 关闭所有容器
`docker stop $(docker ps | awk '{print $1}’)`
## docker创建网络
`docker network create --subnet=`
## 将容器加入已创建网络
`docker network connect somenetwork elasticsearch`