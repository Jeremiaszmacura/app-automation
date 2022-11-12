<hr />

## Build and run jenkins from docker-compose witj public ip adress using ngrok

<hr />

### Up

```sh
docker-compose up -d
```

<hr />

### Down

```sh
docker-compose down
```

<hr />

### Remove volumes

```sh
docker volume rm jenkins-data jenkins-docker-certs
```

<br />
<hr />

## To use Github webhook with Jenkins as localhost use ngrok:

Linux - run ngrok container to map localhost:8080 to ip adress

```sh
docker run -it -e NGROK_AUTHTOKEN=<token> ngrok/ngrok http localhost:8080
```

Windows/MacOS - run ngrok container to map host.docker.internal:8080 to ip adress

```sh
docker run -it -e NGROK_AUTHTOKEN=<token> ngrok/ngrok http host.docker.internal:8080
```

TO get new created ip adress go to

```text
http://localhost:4040
```

<br />
<hr />

## Build and run jenkins manually

<hr />

### Build Jenkins image from Dockerfile

```sh
docker build -t myjenkins-blueocean:2.361.3-1 ./jenkins/Dockerfile
```

<hr />

### Create docker network for container

```sh
docker network create jenkins
```

<hr />

### Run Jenkins container

#### Unix

```sh
docker run --name jenkins-blueocean --restart=on-failure --detach \
--network jenkins --env DOCKER_HOST=tcp://docker:2376 \
--env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
--publish 8080:8080 --publish 50000:50000 \
--volume jenkins-data:/var/jenkins_home \
--volume jenkins-docker-certs:/certs/client:ro \
myjenkins-blueocean:2.361.3-1
```

#### Windows

```sh
docker run --name jenkins-blueocean --restart=on-failure --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.361.3-1
```

<hr />

## Get Jenkins secret

```sh
docker logs jenkins-blueocean
```

<br />
<hr />

## Development setup

<hr />

### Create python virutal enviroment

Create and active virtual enviroment using venv library:

```sh
python3 -m venv .venv
source .venv/bin/activate (Linux)
.venv\Scripts\activate (Windows)
```

In some Windows cases before activating venv:

```sh
Set-ExecutionPolicy Unrestricted -Scope Process
```

<hr />

### Setup flask app

Linux/MacOS

```sh
export FLASK_APP=flaskr/run.py
```

Windows cmd

```sh
set FLASK_APP=flaskr/run.py
```

Windows powershell

```sh
$env:FLASK_APP = "flaskr/run.py"
```

Install app as library in development mode using setuptool

```sh
python -m pip install -e .[dev]
```

Build package (run command each time after changes anmd before building image from Dockerfile)

```sh
python setup.py bdist_wheel
```

<hr />

### Run PosgreSQL database as container

```sh
docker run --name postgres_workshops -e POSTGRES_DB=dev_database -e POSTGRES_USER=dev_user -e POSTGRES_PASSWORD=dev_user -p 5432:5432 -d postgres:14
```

<hr />

### Run flask app

```sh
flask run
```

<hr />

### Run unit tests

```sh
pytest tests
```

<hr />

### Run linter

Pytlint

```sh
python -m pylint flaskr/** tests/**
```

Black check

```sh
python -m black --check .
```

Black fix

```sh
python -m black .
```
