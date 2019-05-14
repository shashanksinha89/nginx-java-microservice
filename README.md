# nginx-java-microservice

## Description
This project contains two micro-services namely `airports` and `countries` which are running in container environment using docker-compose as orchestrator. It uses `nginx` container as reverse proxy to forward traffic to each micro-service through different endpoints.

### Prerequisites

* Docker-compose

## Steps

* Clone this repo (*Note* : This repo contains jar files so it will take sometime download the repo depending upon internet speed)

`# git clone https://github.com/shashanksinha89/nginx-java-microservice.git`

* Once repo is cloned. Run below command to start the complete stack

```
#  cd nginx-java-microservice/

#  docker-compose up -d
```

* Once the stack is up and ready run below command to check health status of microservice. Proceed to next step only when both microservice are in `healthy` state.

```
# docker ps
------
OUTPUT
------

CONTAINER ID        IMAGE                                  COMMAND                  CREATED             STATUS                   PORTS                              NAMES
72b3e47b5778        nginx-java-microservice_loadbalancer   "nginx -g 'daemon of  "   2 minutes ago       Up 2 minutes             80/tcp, 0.0.0.0:8000->8000/tcp     nginx-java-microservice_nginx_1
045310d17391        nginx-java-microservice_airports       "java -Djava.securit  "   2 minutes ago       Up 2 minutes (healthy)   8080/tcp                           nginx-java-microservice_airports_1
2fc0315ebbe3        nginx-java-microservice_countries      "java -Djava.securit  "   2 minutes ago       Up 2 minutes (healthy)   8080/tcp                           nginx-java-microservice_countries_1



```

* As we can see both microservices are running in `healthy` state. Now we can access the microservice's endpoint. Use `curl` or `POSTMAN` to validate the endpoints or via browser

Commandline

```
curl  localhost:8000/airports

curl  localhost:8000/countries
```

Browser

* http://localhost:8000/airports

* http://localhost:8000/countries



#### Components

* Nginx [frontend]

* airports [backend-microservice-1]

* countries [backend-microservice-2]

#### Port Access

* Both microservices can be accessed via nginx only.

* Nginx is exposing port `8000` which can be accessed externally.

* Both microservices exposes port `8080` within the stack for reverse proxy.

* For health check validation, port `8080` is being used from inside of the container.


#### Endpoints

Once the stack is deployed below endpoints are base endpoints that works as expected.

```
curl -I localhost:8000/airports

curl -I localhost:8000/countries

```

As mentioned in requirement `README.md` other endpoints like `/airports/EHAM` for airport  or `/search/ISO CODE` for `countries` microservice are working as expected.

#### Healthcheck

As per requirement below mentioned endpoints are not exposed. This endpoints are used and available to identify container liveness and readiness

* `/health/live`

* `/health/ready`

```
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:8080/health/ready"]
  interval: 60s
  timeout: 10s
  retries: 3
  start_period: 20s
```
Above mentioned is the health check that has been added for both microservices.

Since we are using `docker-compose`, `/health/ready` endpoint is being used to identify container health. Although we can utilize both endpoints if we deploy this tech stack on `kubernetes`. i.e for container readiness use `/health/ready` and for liveness check utilize `/health/live`

#### Dockerfiles

For each service we have different dockerfile. Below are dockerfile names for respective service.

* `nginx`     - `[dockerfile.nginx]`

* `airports`  - `[dockerfile.airports]`

* `countries` - `[dockerfile.countries]`

Refer to `docker-compose.yml` file for complete stack information.



## Future Steps

* Jeknins can be introduced as CI/CD tool to perform rolling updates to microservices.

* This stack can be deployed to `Kubernetes` or any other container orchestrator as well.
