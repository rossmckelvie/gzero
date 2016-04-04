# gzero
GZero simplifies large scale graph-based computing, storage, and machine learning model predictions. All features and labels will be expressed as graph traversals. A user may specify the features necessary to construct a feature vector with the appropriate labels and the framework will provide an easy API for training, evaluating, and performing predictions with stored models.

## Getting Started

#### Titan 1.1
Clone [titan](https://github.com/thinkaurelius/titan/tree/titan11) branch `titan11` and run:  
```mvn clean install -DskipTests=true -Paurelius-release -Dgpg.skip=true```

#### Cassandra & Elastic Search
Titan 1.1 comes with cassandra and elasticsearch. Start cassandra and enable thrift, then start elasticsearch:

```Shell
titan/bin/cassandra -f  
titan/bin/nodetool enablethrift  
titan/bin/elasticsearch
```

#### Gremlin Server

Ensure config in `titan/conf/gremlin-server.yaml` is using the `HttpChannelizer` to enable the REST API. You'll also want to make sure the graph properties include cassandra and elastic search. You can find an example in [conf/gremlin-server.yaml](conf/gremlin-server.yaml)

Now, start the server: `bin/gremlin-server.sh`

#### Running GZero API
Execute
```Shell
sbt run
```
to start the API on port 8080.

## API

### POST `/edge`
Create an edge. Will create verticies if they do not exist.

Request:
```Shell
curl \
	-H 'Content-Type: application/json' \
	-X POST localhost:8080/edge \
	-d '{"label":"drove", "head":{"label":"person","properties" : {"name":"Bonnie"}},"tail":{"label":"vehicle", "properties":{"name":"1932 Ford V-8 B-400 convertible sedan"}}}'
```

### POST `/vertex`
Create a vertex.

Request:
```Shell
curl \
	-H 'Content-Type: application/json' \
	-X POST localhost:8080/vertex \
	-d '{"label":"person","properties":{"name":"Clyde"}}'
```


### GET `/query`
Run a gremlin query against the graph.

Request:
```Shell
curl \
	-H 'Content-Type: application/json' \
	-X GET localhost:8080/query \
	-d '{"gremlin":"g.V().has(\"name\", \"Bonnie\")"}'
```

## Code
If you are interested in contributing and jumping into the code, start with [GZeroService.scala](https://github.com/jamesjgardner/gzero/blob/master/src/main/scala/one/gzero/api/GZeroService.scala)

