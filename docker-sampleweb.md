## Prepare web dir
> cd /opt/sample-web

> pip3 freeze | grep Flask >> requirements.txt

> vim Dockerfile

```
# syntax=docker/dockerfile:1

FROM python:3.8-slim

WORKDIR /main

COPY requirements.txt requirements.txt

ENV FLASK_APP=main.py

RUN pip3 install -r requirements.txt

COPY . .

CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=8080"]

```

## build images
> docker build --tag sampleweb .

## Run container sampleweb

> docker run -d \\ 
> -p 80:8080 \\
> --restart=always \\
> --name sampleweb \\
> sampleweb:latest

