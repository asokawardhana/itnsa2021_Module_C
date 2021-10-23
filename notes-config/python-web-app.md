## Create Web dir
> mkdir -p /opt/sample-web

> vim /opt/sample-web/main.py

```
from flask import Flask
from flask import request
from flask import render_template

sample = Flask(__name__)

@sample.route("/")

def main():
    return render_template("index.html")

if __name__ == "__main__":
    app.sample(host="0.0.0.0", port=8080)

```

> mkdir -p /opt/sample-web/tempates

> vim /opt/sample-web/index.html

```
<html>
<head>
    <title>Sample app</title>
</head>
    <body>
        <h1>You are calling me from {{request.remote_addr}}</h1>
    </body>
</html>

```

## install python package
> apt install python3-virtualenv python3-venv virtualenv

## Create virtual env
> python3 -m venv /opt/env/myenv

> cd /opt/env

> . myenv/bin/activate

## Install Flash & test run
> pip3 install Flask

> export FLASK_APP=/opt/sample-web/main.py

> export FLASK_RUN_PORT=8080

> export FLASK_RUN_HOST=0.0.0.0

> flask run
