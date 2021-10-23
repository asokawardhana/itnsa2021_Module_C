## Docker Registry
> \# docker pull registry:2

```
# docker run -d \
-p 5000:5000 \
--restart=always \
--name registry \
-v /mnt/registry:/var/lib/registry \
registry:2
```

> \# docker pull python:3.8-slim

> \# docker tag python:3.8-slim localhost:5000/python:3.8-slim

> \# docker push localhost:5000/python:3.8-slim

> \# docker pull nginx:latest

> \# docker tag nginx:latest localhost:5000/webserver.base

> \# docker push localhost:5000/webserver.base


## Test insecure pull images on Docker Host

> /# vim /etc/docker/daemon.json

```
  "insecure-registries" : ["myregistrydomain.com:5000"]
```

> \# docker pull 192.168.41.108:5000/webserver.base

## Docker Registry with ssl
> \# mkdir -p /opt/registry/certs

> \# cd /opt/registry/

> \# openssl req -newkey rsa:4096 -nodes -sha256 -keyout certs/registry.itnsa2021.id.key -addext "subjectAltName = DNS:registry.itnsa2021.id" -x509 -days 365 -out certs/registry.itnsa2021.id.crt


### on Docker Server :
> \# mkdir -p /etc/docker/certs.d/registry.itnsa2021.id:5000

> \# cp /opt/registry/certs/registry.itnsa2021.id.crt /etc/docker/certs.d/registry.itnsa2021.id:5000/ca.crt

> \# cp /opt/registry/certs/registry.itnsa2021.id.crt /usr/local/share/ca-certificates/ca.crt

> \# update-ca-certificates

``` 
# docker run -d \
--restart=always --name registry \
-v "$(pwd)"/certs:/certs -e REGISTRY_HTTP_ADDR=0.0.0.0:5000 \
-e REGISTRY_HTTP_TLS_CERTIFICATE=/opt/registry/certs/registry.itnsa2021.id.crt \
-e REGISTRY_HTTP_TLS_KEY=/opt/registry/certs/registry.itnsa2021.id.key \
-p 5000:5000 \
registry:2 
```

### On Docker Host/Client :
-  download registry.itnsa2021.id.crt from docker registry server
> \# mv registry.itnsa2021.id.crt ca.crt

> \# cp ca.crt /etc/docker/certs.d/registry.itnsa2021.id:5000/ca.crt

> \# cp ca.crt /usr/local/share/ca-certificates/ca.crt

> \# update-ca-certificates

> \# docker pull registry.itnsa2021.id:5000/nginx:latest

