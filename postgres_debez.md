Run in local terminal (or whereever you are hosting docker containers) 

### Initiate Zookeeper ## 
```
docker run -it --rm --name zookeeper -p 2181:2181 -p 2888:2888 -p 3888:3888 debezium/zookeeper:0.10
```
