# Reverse Proxy

An **Nginx** and **Let's Encrypt** container package used as a proxy to deploy multiple containerized web applications on a single server.

Based on this article: https://linuxhandbook.com/nginx-reverse-proxy-docker/ (author: [Debdut Chakraborty](https://linuxhandbook.com/author/debdut/)).

## Useful Articles
It is recommended that you read these articles first.
1. https://linuxhandbook.com/nginx-reverse-proxy-docker/
2. https://olex.biz/2019/09/hosting-with-docker-nginx-reverse-proxy-letsencrypt/

## Used Images
1. https://hub.docker.com/r/jwilder/nginx-proxy
2. https://hub.docker.com/r/jrcs/letsencrypt-nginx-proxy-companion

## Prerequisites
**Docker** and **Docker Compose** must be installed on your host.

## Deploying a Reverse Proxy

#### Step 1
Create a network `reverse-proxy-net`:
```
docker network create reverse-proxy-net
```
#### Step 2
Rename the `sample.env` file to `.env`. Set the `DEFAULT_EMAIL` variable to the desired value.
#### Step 3
Deploy these two containers (Nginx and Let's Encrypt) using the following command:
```
docker compose up -d
```
You can stop containers with the following command:
```
docker compose down
```

## Deploying a Web Application Using a Reverse Proxy
There are two things to consider when deploying a web application:
1. The frontend container that communicates with the reverse proxy must use the external network `reverse-proxy-net`.
2. The container that'll serve the frontend will need to define two environment variables:
    - `VIRTUAL_HOST`: for generating the reverse proxy config
    - `LETSENCRYPT_HOST`: for generating the necessary certificates

#### Example
Start an Nginx web server on sub.domain.com using a reverse proxy.
```
docker run --rm --name nginx-dummy -e VIRTUAL_HOST=sub.domain.com -e LETSENCRYPT_HOST=sub.domain.com --network reverse-proxy-net -d nginx:latest
```
