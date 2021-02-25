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

## Configure Your App
* Refer [this](https://www.elastic.co/guide/en/apm/agent/python/current/django-support.html) page to setup and [this](https://www.elastic.co/guide/en/apm/agent/python/current/configuration.html#config-debug) page to configure.
* Install python agent: ```pip install elastic-apm```
* Add ```elasticapm.contrib.django``` to ```INSTALLED_APPS``` in your settings
* Add ```elasticapm.contrib.django.middleware.TracingMiddleware``` at first to ```MIDDLEWARE``` in your settings
* Example config in your Django settings:

    ```python
    # Elastic APM
    APM_ENABLED = env.bool('APM_ENABLED', default=True)
    if APM_ENABLED:
        INSTALLED_APPS.append('elasticapm.contrib.django')
        # middleware should be first for best results
        MIDDLEWARE.insert(0, 'elasticapm.contrib.django.middleware.TracingMiddleware')
        ELASTIC_APM = {
            "DEBUG": True,  # monitor when app is in DEBUG mode
            'SERVICE_NAME': env('APM_SERVICE_NAME', default='My App'),  # name for your app
            # token is configured in APM server. Do not change
            'SECRET_TOKEN': env('APM_SECRET_TOKEN', default='somerandomstring'), # this is token you put in apm-server.yml file
            'SERVER_URL': env('APM_SERVER_URL', default='http://localhost:8200'), # this is kibana endpoint
            'CAPTURE_BODY': 'all'
        }
    ```
* Run ```python manage.py elasticapm check``` to check if everything is configured properly.
* Run ```python manage.py elasticapm test``` to send sample error to APM

Once your app is up and running, visit your ```SERVER_URL``` and navigate to APM


Note:
* If docker-compose fails with message: ```ERROR: for apm-server  Container "*******" is unhealthy.
ERROR: Encountered errors while bringing up the project.```, increase healthcheck retries count for kibana service in docker-compose file.

Version:
    - elasticsearch:7.1.1
    - kibana:7.1.1

Inspired by: https://github.com/yidinghan/eak
