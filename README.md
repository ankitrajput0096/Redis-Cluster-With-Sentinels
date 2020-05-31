# Redis-Cluster-With-Sentinel
**Redis cluster with Docker Compose** 

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development purposes. See running for notes on how to run the project on a system.

### Prerequisites

1. Clone the project to your local environment:
    ```
    git clone https://github.com/ankitrajput0096/Redis-Cluster-With-Sentinels
    ```

2. You need Docker to be installed:

    #### Windows:
    https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe
    
    #### Mac:
    https://download.docker.com/mac/stable/Docker.dmg
    
    #### Ubuntu:
    https://docs.docker.com/install/linux/docker-ce/ubuntu/

## Redis Cluster Configurations Details

There are following services in the cluster,

* master: Master Redis Server
* slave:  Slave Redis Server
* sentinel: Sentinel Server


The sentinels are configured with a "mymaster" instance with the following properties -

```
sentinel monitor mymaster redis-master 6379 2
sentinel down-after-milliseconds mymaster 5000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 5000
```

The details could be found in sentinel/sentinel.conf

The default values of the environment variables for Sentinel are as following

* SENTINEL_QUORUM: 2
* SENTINEL_DOWN_AFTER: 5000
* SENTINEL_FAILOVER: 5000

## Installing and Running

Build services
```
docker-compose build
```
running services
```
docker-compose up
```
Check the status of redis cluster
```
docker-compose ps
```
The result is 
```
          Name                        Command               State                 Ports               
------------------------------------------------------------------------------------------------------
redis_cluster_master_1     docker-entrypoint.sh redis ...   Up      6379/tcp                          
redis_cluster_sentinel_1   entrypoint.sh                    Up      0.0.0.0:10001->26379/tcp, 6379/tcp
redis_cluster_slave_1      docker-entrypoint.sh redis ...   Up      6379/tcp  
```

#### NOTE : Refer the below configuration of cluster as SENTINEL_QUORUM value is 2.
Scale out the instance number of sentinel and number of slaves/Replica

```
docker-compose up --scale sentinel=3 --scale slave=4
```

Check the status of redis cluster

```
docker-compose ps
```

The result is 

```
          Name                        Command               State                 Ports               
------------------------------------------------------------------------------------------------------
redis_cluster_master_1     docker-entrypoint.sh redis ...   Up      6379/tcp                          
redis_cluster_sentinel_1   entrypoint.sh                    Up      0.0.0.0:10001->26379/tcp, 6379/tcp
redis_cluster_sentinel_2   entrypoint.sh                    Up      0.0.0.0:10002->26379/tcp, 6379/tcp
redis_cluster_sentinel_3   entrypoint.sh                    Up      0.0.0.0:10003->26379/tcp, 6379/tcp
redis_cluster_slave_1      docker-entrypoint.sh redis ...   Up      6379/tcp                          
redis_cluster_slave_2      docker-entrypoint.sh redis ...   Up      6379/tcp                          
redis_cluster_slave_3      docker-entrypoint.sh redis ...   Up      6379/tcp                          
redis_cluster_slave_4      docker-entrypoint.sh redis ...   Up      6379/tcp  
```

For pausing master redis server.
```
docker-compose pause master
```
For unpausing master redis server.
```
docker-compose unpause master
```
And get the sentinel infomation with following commands

```
redis-cli -p 10001 -h 127.0.0.1 sentinel get-master-addr-by-name mymaster
```

### Getting all required information from redis sentinels about this redis cluster
#### NOTE: To run all the below commands, install redis-tools in your terminal using -> sudo apt install redis-tools

|Command                                                      |    Description                                           |
--------------------------------------------------------------|----------------------------------------------------------|	                                                  
|redis-cli -p 10001 -h 127.0.0.1 sentinel masters             |   Show a list of all monitored masters + state           |
|redis-cli -p 10001 -h 127.0.0.1 sentinel master mymaster     |   Show the state of a specified master                   |
|redis-cli -p 10001 -h 127.0.0.1 sentinel slaves mymaster     |   Show the slaves of the master + state                  |
|redis-cli -p 10001 -h 127.0.0.1 sentinel sentinels mymaster  |   Show the Sentinel instances for this master + state    |
|redis-cli -p 10001 -h 127.0.0.1 sentinel get-master-addr-by-name mymaster | Return ip and port of the master. While a failover is in progress the ip and port of the promoted slave are returned |
|redis-cli -p 10001 -h 127.0.0.1 sentinel reset mymaster | Reset the masters with a matching name. Clears previous state for the master, removes every slave and sentinel discovered. A fresh discovery is started. |
|redis-cli -p 10001 -h 127.0.0.1 sentinel failover mymaster   |   Force a failover, as if the master was not reachable   |
|redis-cli -p 10001 -h 127.0.0.1 sentinel ckquorum mymaster   |   Check if the current Sentinel configuration is able to reach quorum and majority |
|redis-cli -p 10001 -h 127.0.0.1 sentinel flushconfig         |	  Force Sentinel to rewrite it's configuration on disk   |
|redis-cli -p 10001 -h 127.0.0.1 sentinel remove mymaster     |	  Remove a specified master from monitoring              |

## Built With

* [Docker](https://www.docker.com/) - For containerization of application
* [Redis](https://redis.io/) - For Redis
* [Redis Sentinel](https://redis.io/topics/sentinel) - For Redis Sentinel

## Contributing

If you have any improvement suggestions please create a pull request and I'll review it.


## Authors

* **Ankit Rajput** - *Initial work* - [Github](https://github.com/ankitrajput0096)

## License

This project is licensed under the MIT License

