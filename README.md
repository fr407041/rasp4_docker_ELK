### How to launch docker ELK in raspberry4

###### Index 

1. [Prepared Docker Image](#Docker_Image)
2. [Docker Launch Command](#launch)

----
<a name="Docker_Image"/>

###### Prepared Docker Image

* Thanks for `karlhendrik` to create great docker ELK image in raspberry4, I have spend a lot of time to find it.

1. [karlhendrik/elasticsearch](https://hub.docker.com/r/karlhendrik/elasticsearch)
```
docker pull karlhendrik/kibana:latest
```

2. [karlhendrik/kibana](https://hub.docker.com/r/karlhendrik/kibana)
```
docker pull karlhendrik/kibana:v7.4.1
```

----
<a name="launch"/>

###### Docker Launch Command