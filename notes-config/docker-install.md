## Setup Repository
> apt-get install apt-transport-https ca-certificates curl gnupg lsb-release

> sudo mkdir -p /etc/apt/keyrings

>  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

>  echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

## Install docker engine
> apt update

> apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin

> docker run hello-world.

## Run docker from non-root user
> groupadd docker

> usermod -aG docker support

## Install docker compose
>  sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

 
