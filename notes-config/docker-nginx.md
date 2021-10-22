## Farm1 
> \# mkdir -p /opt/farm_1/html

> \# mkdir -p /opt/farm_1/conf

> \# vim /opt/farm_1/html/public.html

```
<h1> Public Web Page </h1><br>
<br>
Welcome to ITNSA LKSN 2021
```

> \# vim /opt/farm_1/conf/default.conf

```
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  public.html;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}

```

```
# docker run -d \
-v /opt/farm_1/html:/usr/share/nginx/html \
-v /opt/farm_1/conf:/etc/nginx/conf.d/ \
--restart=always \
--name farm-1 \
nginx:latest
```

## Farm2
> \# mkdir -p /opt/farm_2/html

> \# mkdir -p /opt/farm_2/conf

> \# vim /opt/farm_2/html/external.html

```
<h1> External Web Page </h1><br>
<br>
Welcome to ITNSA LKSN 2021
```

> \# vim /opt/farm_2/conf/default.conf

```
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  external.html;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}

```

```
# docker run -d \
-v /opt/farm_2/html:/usr/share/nginx/html \
-v /opt/farm_2/conf:/etc/nginx/conf.d/ \
--restart=always \
--name farm-2 \
nginx:latest
```

## Farm-lb
> \# mkdir -p /opt/farm_lb/conf

> \# vim /opt/farm_lb/conf/default.conf

```
upstream farm {
    least_conn;
    server 172.17.0.4;
    server 172.17.0.5;
}

server {
    listen 443 ssl;
    server_name farm.itnsa2021.id;
    ssl_certificate /etc/nginx/certs/farm.itnsa2021.id.crt;
    ssl_certificate_key /etc/nginx/certs/farm.itnsa2021.id.key;

    location / {
        proxy_pass https://farm;
    }
}

```

> \# mkdir -p /opt/farm_lb/certs

> \# cd /opt/farm_lb

> \# openssl req -newkey rsa:4096 -nodes -sha256 -keyout certs/farm.itnsa2021.id.key -addext "subjectAltName = DNS:farm.itnsa2021.id" -x509 -days 365 -out certs/farm.itnsa2021.id.crt

> \# cd 

```
# docker run -d \
 -p 443:443 \
 --restart=always \
 --name farm-lb \
 nginx:latest
 ```

> \# docker cp /opt/farm_lb/conf/default.conf farm-lb:/etc/nginx/conf.d/default.conf

> \# docker cp /opt/farm_lb/certs farm-lb:/etc/nginx/

> \# docker restart farm-lb
