# Docker 

We will create and deploy a simple flask application using Docker.

## Setup

Pull and run [hello-world](https://hub.docker.com/_/hello-world/) container from Docker Hub.

```bash
docker run hello-world
```
This simple container returns Hello from Docker! to your screen. While the command is simple, notice in the output the number of steps it performed. The docker daemon searched for the hello-world image, didn't find the image locally, pulled the image from a public registry called Docker Hub, created a container from that image, and ran the container for you.


Run the following command to take a look at the container image it pulled from Docker Hub:

```bash
docker images
```

Build a Docker image that's based on a simple python application.

```bash
mkdir app && cd app
```

Create a Dockerfile:
```bash
cat > Dockerfile <<EOF
FROM python:3.7

RUN mkdir /app
WORKDIR /app
ADD . /app/
RUN pip install -r requirements.txt

EXPOSE 5000
CMD ["python", "/app/main.py"]
EOF
```

Create a python application:

```bash
cat > main.py <<EOF

from flask import Flask
import os
app = Flask(__name__)

@app.route("/")
def hello():
    hostinfo = os.uname()
    return "Hello from "+ str(hostinfo)

if __name__ == "__main__":
    app.run(host='0.0.0.0')
EOF
```
Create requirements.txt:
```bash
cat > requirements.txt <<EOF
Flask
EOF
```

Build the container:
```bash
docker build -t app:0.1 .
```

```bash
docker images
```

## Run

```bash
docker run -d -p 5000:5000 app:0.1

curl http://localhost:5000
```

Stop and remove the container

```bash
docker stop [container id] && docker rm [container id]
```

Run the following command to start the container in the background

```bash
docker run -p 5000:5000 --name my-app -d app:0.1

docker ps
```

## Troubleshooting

```bash
docker logs [container_id]

docker logs -f [container_id]

docker exec -it [container_id] bash

docker inspect [container_id]

```
