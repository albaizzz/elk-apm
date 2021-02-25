# [albaizzz - Elastic APM](https://www.elastic.co/solutions/apm)

Elastic APM, a open source application performance monitoring tool. This repo consists of Dockerfile and docker compose file to setup APM.

![](https://www.elastic.co/assets/blt55027d175d758616/animation-apm-app.gif)

## Bring up Docker
* Change secret token for your app. In ```apm-server.yml``` file, change ```somerandomstring```. Note this token.
* build docker image: ```docker build -t elastic-apm-server:7.1.1 .```
* run docker compose: ```docker-compose up -d```.  This will take some time as it waits for elastic-search and kibana service to start.
* check if all containers are up.  ```docker-compose ps```
* Kibana will run on port:8200 and apm-server on port:5601. 
* port 8200 is used to visualize data, and port 5601 is to ping data from your app

Once your app is up and running, visit your ```SERVER_URL``` and navigate to APM


Note:
* If docker-compose fails with message: ```ERROR: for apm-server  Container "*******" is unhealthy.
ERROR: Encountered errors while bringing up the project.```, increase healthcheck retries count for kibana service in docker-compose file.

Version:
    - elasticsearch:7.1.1
    - kibana:7.1.1

Inspired by: https://github.com/yidinghan/eak
