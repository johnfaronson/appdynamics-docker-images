# Building Docker with DB Agent

## Pre-Requisites

- Docker installed.
- Download [DB Agent](https://download.appdynamics.com/download/#version=&apm=db&os=linux)


## Steps

Follow these steps to build a new container with the latest dbagent version.

- Download DB agent from [AppDynamics Download Portal](https://download.appdynamics.com/download/#version=&apm=db&os=linux) into the current directory. 

```
(master)$ ls
Dockerfile			README.md			docker-compose.yml
dbagent-4.5.4.885.zip	appdynamics.env			env

```

- set the environment variable 
  -  `APPD_AGENT_VERSION` to the version of machine agent 
  -  `APPD_AGENT_SHA256` to sha256 of the newly packaged MachineAgent zip file.
  -  `APPD_AGENT_TAG` shortened form the version to be used as the image tag
  
```
(master)$ export APPD_AGENT_TAG=4.5.4

(master)$ export APPD_AGENT_VERSION=4.5.4.885

(master)$ shasum -a 256 dbagent-4.5.4.885.zip 
2a511b3e0051e8d4bd958e6bf053d426698393fd8df00ed867dd49c27cdcf2b3  dbagent-4.5.4.885.zip

(master)$ export APPD_AGENT_SHA256=2a511b3e0051e8d4bd958e6bf053d426698393fd8df00ed867dd49c27cdcf2b3

```

- Build the image by doing `docker-compose build`

```
(master)$ docker-compose build

....
Removing intermediate container a3a73c8622c8
 ---> 30beb29611f7
Successfully built 30beb29611f7
Successfully tagged jfaronson/appdynamics-dbagent:4.5.4

```


## Running the container

- Fill in the [controller configuration](https://docs.appdynamics.com/display/PRO45/Standalone+Machine+Agent+Configuration+Properties) in the file `env`

You may include any other AppDynamics configuration environment variables you wish to include.  


```
$ vim env 

APPDYNAMICS_AGENT_ACCOUNT_ACCESS_KEY=acesssKey
APPDYNAMICS_AGENT_ACCOUNT_NAME=customer1
APPDYNAMICS_CONTROLLER_HOST_NAME=controller.e2e.appd-test.com
APPDYNAMICS_CONTROLLER_PORT=8090
APPDYNAMICS_AGENT_APPLICATION_NAME=app
APPDYNAMICS_AGENT_NODE_NAME=node
APPDYNAMICS_AGENT_TIER_NAME=tier

```



- Just do a `docker run`

```
$ docker run --env-file env  jfaronson/appdynamics-dbagent:4.5.4 

Using Java Version [1.8.0_181] for Agent
Using Agent Version [Machine Agent v4.5.4.885 GA Build Date 2018-07-25 17:57:44]
[INFO] Agent logging directory set to: [/opt/appdynamics]

```
