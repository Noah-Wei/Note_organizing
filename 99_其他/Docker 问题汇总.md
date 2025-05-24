# Docker 问题汇总

- 端口问题

```
docker容器允许外部访问
	通过IP:端口 的方式访问
```

```
    ports:
      - 8081:80
      宿主机端口：容器端口
```

- 怎么看我的docker镜像是基于什么系统的？


```
docker inspect 镜像名或ID

docker run --rm 镜像名 cat /etc/os-release
```

- 默认存储卷


```
/var/lib/docker/volumes
```

- 将本地文件复制到 Docker 容器内

```
docker cp <本地文件路径> <容器名或ID>:<容器内目标路径>
```

