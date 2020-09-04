# docker 安装 redis

```
docker pull redis

docker run --name redis-dev -d redis redis-server --appendonly yes

docker ps 看启动成功了没

docker exec -it id bash

redis-cli

ping -> Pong 安装成功
```

容器结束就删除
--rm

注意点:
后台运行配置项
daemonize yes

默认是只能本机连接
删除 bind 127.0.0.1 或者添加 ip

启动配置文件

```
docker run -d --privileged=true -p 6379:6379 -v /docker/redis/redis.conf:/etc/redis/redis.conf redis redis-server /etc/redis/redis.conf --appendonly yes
```

docker run -d --privileged=true -p 6379:6379 -v redis/redis.conf:/etc/redis/redis.conf redis redis-server /etc/redis/redis.conf --appendonly yes
调试时候, windows 使用 another redis desktop manager 连接

2 个 docker 容器网络是不通的

需要加 `--link`

过滤不注释
cat redis.conf | grep -v "#" | grep -v "^\$"

daemonize no ：redis 默认是不作为守护进程使用的，这也就是说为什么在你不修改配置文件时直接使用 redis-server /redis/redis.conf 启动 redis 会直接显示一个服务，你在这个终端就没有办法操作其他，只能新开一个终端来连接 redis
requirepass foobared ：redis 默认是没有密码连接的，但是为了安全密码还是需要设置的
bind 127.0.0.1：这个配置项一般是直接注释掉的，这个配置开启后就只有本机可以连接 redis
protected_mode no

--restart 容器退出一直重启
-v 挂硬盘
-p 端口号
--link 容器间网络联通
--net 建网络
