# docker容器运行后退出

执行 `docker run image-id bash` 后容器会退出, docker内部是pid1作为标志

解决方案:

1. shell执行一个死循环

```
docker run -d centos /bin/bash -c "while true;do echo hello docker;sleep 1;done"
```

2. 交互式: 

```
docker run -i [CONTAINER_NAME or CONTAINER_ID]
```

如果要退出的话使用 `ctrl + p + q` 别使用 `ctrl + c`

3. docker logs

