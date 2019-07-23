# Update Log

Docker Swarm 的 overlay network的dns具备服务发现和自动负载均衡功能, 不需要consul

# Consul Registrator

## Consul 

```
$ docker pull consul
```


## Registrator 

```
$ docker pull gliderlabs/registrator:latest
```


```
$ docker run -d \
    --name=registrator \
    --net=host \
    --volume=/var/run/docker.sock:/tmp/docker.sock \
    gliderlabs/registrator:latest \
      consul://localhost:8500
```

# 使用docker stack 来为docker集群启动Registrator

```
docker stack deploy -c docker-compose.yml consul
docker stack ps consul
docker service ls
docker service ps consul_consulserver
```



关闭stack
```
docker stack rm consul
```

# TODO 

consul限定在swarm leader上部署

不然, 在leader上不能localhost访问......
