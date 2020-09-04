# redis 持久化

1. RDB
   特定时间间隔保存数据快照

2. AOF
   把每条修改数据的命令存起来, 重启 redis AOF 里面命令会重新执行一次

启动 aof:
appendonly yes -> appendonly.aof
appendfsync always/no/everysec(default)

备份:
定时任务

1. 每小时每天创建一个快照, 保存在不同文件夹
2. 删掉旧的
3. 上传到其他服务器保存
