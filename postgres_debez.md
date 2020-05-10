Run in local terminal (or whereever you are hosting docker containers) 

https://info.crunchydata.com/blog/postgresql-change-data-capture-with-debezium

### Initiate Zookeeper ## 
First we need to start zookeeper which is a distributed configuration store. Kafka uses this to keep information about which Kafka node is the controller, it also stores the configuration for topics. This is where the status of what data has been read is stored so that if we stop and start we don’t lose any data.

```
docker run -it --rm --name zookeeper -p 2181:2181 -p 2888:2888 -p 3888:3888 debezium/zookeeper:0.10
```
### Open Kafka ### 
```
docker run -it --rm --name kafka -p 9092:9092 --link zookeeper:zookeeper debezium/kafka:0.10
```

### Create mock Postgres DB ### 

```
#we will need to customize the postgresql.conf file to ensure wal_level=logical
cat << EOF > pgconf/postgresql.conf
# here are some sane defaults given we will be unable to use the container
# variables
# general connection
listen_addresses = '*'
port = 5432
max_connections = 20
# memory
shared_buffers = 128MB
temp_buffers = 8MB
work_mem = 4MB
# WAL / replication
wal_level = logical
max_wal_senders = 3
# these shared libraries are available in the Crunchy PostgreSQL container
shared_preload_libraries = 'pgaudit.so,pg_stat_statements.so'
EOF
```
