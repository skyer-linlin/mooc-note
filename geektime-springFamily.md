## 24 docker 辅助开发

`docker run [options] image [command] [arg...]`

- -d，后台运行容器
- -e，设置环境变量
- --expose，-p 宿主端口：容器端口
- -name，指定容器名称
- --link，链接不同容器

`docker run --name mongo -p 27017:27017 -v ~/docker-data/mongo:/data/db -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=admin -d mongo`

### 登录到容器中

`docker exec -it mongo bash`

### 通过 shell 连接 MongoDB

`mongo -u admin -p admin`

## 26 redis

`docker ps -a` 展示之前启动过的容器
`docker start {name}` 按名字重启之前的容器
