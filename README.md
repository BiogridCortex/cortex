# Cortex
Orchestration of the Docker containers involved in Biogrid Cortex using Docker Compose.

No effort has been done to secure access to the various processes.

## Usage

### Starting Biogrid Cortex
```docker-compose --x-networking up``` to run in foreground.
```docker-compose --x-networking up -d``` to run as daemon.

### Stopping Biogrid Cortex
```docker-compose stop```

## Howto
This assumes that you're on OS X or Windows.

### Node-RED
Node-RED is used for data stream processing, ie. to turn the stream of sensor readings into time series data for MongoDB. Note that the time series data will store max one sensor reading value per second.

Open Node-RED in browser to look at / modify flows file:

 ```open http://`docker-machine ip default`:1880```

The default flows file generates a constant stream of mock data for two sensors. Click the README node in Node-RED for more info.

### MongoDB
The MongoDB container exposes port 27017 on the Docker host. A separate data volume container is created by Docker Compose to persist MongoDB data even if the MongoDB container needs to be recreated.

Use [MongoDB Compass](https://www.mongodb.com/products/compass) or MongoDB shell to inspect the data.

Note that the timeseries data can be queried by day and the projection can be narrowed to hour, minutes, seconds.

#### Timeseries query examples in MongoDB Shell
To connect using MongoDB Shell:

```mongo --host `docker-machine ip default` ```

(this requires Mongo to be installed on your host)

Use the Biogrid database:

```use biogrid```

To get all stored sensor readings for the RH sensor with ID 81772309-6FFF-4886-9977-DA15AD34C263 for a specific day:

```
db.sensors.find({type: "rh", day: "2016-02-04", sensor_id:"81772309-6FFF-4886-9977-DA15AD34C263"}).pretty()
```

To narrow the query to all readings at 10:30 on the same day, use projection:

```
db.sensors.find({type: "rh", day: "2016-02-04", sensor_id:"81772309-6FFF-4886-9977-DA15AD34C263"}, {"values.10.30":1, _id:0}).pretty()
```
