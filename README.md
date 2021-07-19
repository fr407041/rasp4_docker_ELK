# How to launch docker ELK in raspberry4

#### Index 

1. [Install 64bit Raspberry](#install)
2. [Prepared Related Docker Image](#Docker_Image)
3. [docker-compose.yml Command](#launch)
4. [Check](#check)
5. [kibana setting](#kibana)

----

<a name="install"/>

#### Install 64bit Raspberry

* You must download 64 bit raspberry os from [https://downloads.raspberrypi.org/raspios_arm64/images/raspios_arm64-2020-08-24/2020-08-20-raspios-buster-arm64.zip](https://downloads.raspberrypi.org/raspios_arm64/images/raspios_arm64-2020-08-24/2020-08-20-raspios-buster-arm64.zip)

* utilize [Rufus](https://rufus.ie/downloads/) to install above image.

<a name="Docker_Image"/>

#### Prepared Related Docker Image

* Dowload Elasticsearch Image from [here](https://www.docker.elastic.co/r/elasticsearch) ( I use [7.13.2-arm64](https://www.docker.elastic.co/r/elasticsearch/elasticsearch:7.13.2-arm64) )

* Dowload kibana Image from [here](https://www.docker.elastic.co/r/kibana) ( I use [7.13.2-arm64](https://www.docker.elastic.co/r/kibana/kibana:7.13.2-arm64) )

----
<a name="launch"/>

#### docker-compose.yml Command

* You can eliminate `deploy section`, if you don't use docker swarm mode.

```
version: '3.6'
networks:
  esnet:
    driver: "overlay"  
services:
  es00:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.13.2-arm64
    hostname: es00
    environment:
      - cluster.name=docker-cluster
      - node.name=es00
      - node.data=true
      - node.master=true
      - bootstrap.memory_lock=true
      - discovery.type=single-node
      - "ES_JAVA_OPTS=-Xmx2g -Xms2g"
      - xpack.security.enabled=false
      - xpack.monitoring.collection.enabled=true   
    volumes:
      #- /mnt/linux/Docker/ELK/elastic_storage:/usr/share/elasticsearch/data
      - es00:/usr/share/elasticsearch/data
    networks:
      - esnet
    ports:
      - 9200:9200
      - 9300:9300      
    deploy:
      replicas: 1
      placement:
        constraints:
         - node.labels.role == elasticsearch
  kibana:
    image: docker.elastic.co/kibana/kibana:7.13.2-arm64
    hostname: "docker_{{.Node.Hostname}}-{{.Service.Name}}"
    environment:
       - ELASTICSEARCH_HOSTS=http://es00:9200
    ports:
      - 5601:5601
    networks:
      - esnet
    depends_on:
      - es00
    deploy:
      placement:
        constraints:
          - node.labels.role == elasticsearch
 
volumes:
  es00: {}
```

<a name="check"/>

#### Check

1. Check elasticsearch , please click `http://localhost:9200` and show below means it's work!

![](tmp/check_elastic_live.png)

2. Check kibana, please click `http://localhost:5601` and show below means it's work!

![](tmp/check_kibana_live.png)

<a name="kibana"/>

#### kibana setting

1. How to setting Index template by kibana
![](https://i.imgur.com/4w6OPkL.png)
![](https://i.imgur.com/OFXvHX9.png)
![](https://i.imgur.com/1iIR13q.png)
![](https://i.imgur.com/kWKwr8B.png)
![](https://i.imgur.com/jgLmg3v.png)
![](https://i.imgur.com/RdzQy8Z.png)


3. 

###### tags: `ELK` `Single Node`