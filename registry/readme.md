# Docker Registry

HTTP协议的Docker Registry


# 安装方法

数据存储在`/data/tools/docker-registry/docker-registry`中.

安装`docker-compose up -d`

`http://10.249.182.84:5000`是registry服务

`http://10.249.182.84:8080`是registry web ui




# 使用方法

由于HTTP无加密, 因此, 如果需要使用这个私服, 需要在客户端的`/etc/docker/daemon.json`添加以下内容

```
{
    "insecure-registries" : ["10.249.182.84:5000"]
}
```
后, 重启docker
```
sudo service docekr restart
```

就能使用push和pull了
```
docker pull 10.249.182.84:5000/python
docker push 10.249.182.84:5000/镜像名
```


# Web 管理

10.249.182.84:8080