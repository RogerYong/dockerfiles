# registry

## 运行registry及Web UI

### 方法1. 强制HTTP(最简单)

  服务端
  ```
  docker run -d \
      -p 5000:5000 \
      --restart=always \
      -v /media/ices/327E75477E7504BF/docker/registry:/var/lib/registry \
      --name registry-srv \
      registry:2
  ```

  [Web UI](https://github.com/mkuchin/docker-registry-web)
  ```
  docker run -d \
    -p 8080:8080 \
    --name registry-web \
    --link registry-srv \
    --restart=always \
    -e REGISTRY_URL=http://registry-srv:5000/v2 \
    -e REGISTRY_NAME=registry-srv:5000 \
    hyper/docker-registry-web
  ```



  客户端在daemon.json中添加, 重启docker

  ```
  {
    "insecure-registries" : ["10.0.244.12:5000"]
  }
  ```

### 方法2. 自签名证书(在不重启docker的情况下使用, 麻烦)

```
$ mkdir -p certs

$ openssl req \
  -newkey rsa:4096 -nodes -sha256 -keyout certs/domain.key \
  -x509 -days 365 -out certs/domain.crt
```

域名是 hub.docker.ices.com

详细信息如下: 

Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:Guangdong
Locality Name (eg, city) []:Shenzhen
Organization Name (eg, company) [Internet Widgits Pty Ltd]:HITsz
Organizational Unit Name (eg, section) []:ices
Common Name (e.g. server FQDN or YOUR name) []:hub.docker.ices.com    
Email Address []:abc@gmail.com   


```
docker run -d \
  --restart=always \
  --name docker-registry \
  -v "$(pwd)"/certs:/certs \
  -v /media/ices/327E75477E7504BF/docker/registry:/var/lib/registry \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  -p 5000:5000 \
  registry:2

```

客户端修改/etc/hosts
```
10.0.244.12   hub.docker.ices.com 
```
证书拷贝
```
cd /etc/docker/certs.d
mkdir hub.docker.ices.com:5000
```
copy server's domain.crt  to /etc/docker/certs.d/hub.docker.ices.com:5000/ca.crt


## 使用registry
(如果是方法1,下面的命令改成ip...)

给已有Image打上新的标签
```
docker tag <imagename> hub.docker.ices.com:5000/<imagetag>
```

把image push 到仓库中
```
docker push hub.docker.ices.com:5000/<imagetag>
```

从仓库中拉取镜像
```
docker pull hub.docker.ices.com:5000/<imagetag>
```
