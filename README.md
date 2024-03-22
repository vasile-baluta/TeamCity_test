# TeamCity_test
Test Teamcity with BitBucket as VCS source.

##Prerequisites 

Install docker

## Run
To start service
```
docker compose up -d
```

To shut down
```
docker compose down
```

## Issues
### GCP
Starting Bitbucket standalone works but not in compose since folder ./bitbucket/data gets root owner in compose but another id when started with

```
docker run -d     --name bitbucket1     --restart always     -p 7990:7990     -p 7999:7999     -e SERVER_PROXY_NAME=localhost     -e SERVER_PROXY_PORT=7990     -e JVM_MINIMUM_MEMORY=512m     -e JVM_MAXIMUM_MEMORY=2048m     -e JVM_SUPPORT_RECOMMENDED_ARGS="-XX:G1HeapRegionSize=4m -XX:MaxGCPauseMillis=200 -XX:+UseG1GC"     -v "$(pwd)/bitbucket1/data:/var/atlassian/application-data/bitbucket"     atlassian/bitbucket-server
```
The short term solution for now is to for example run:
```
docker compose down
sudo chown -R 2003:2003 bitbucket/data/
docker compose up -d
```
assuming that 2003 is the id that owns the folder when starting with docker run and not docker compose.

