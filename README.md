# Cortex
Orchestration of the Docker containers involved in Biogrid Cortex using Docker Compose.

## Usage
```docker-compose --x-networking up``` to run in foreground.
```docker-compose --x-networking up -d``` to run as daemon.

## Flows file
Node-RED uses biogrid-cortex-flows.json as custom flows file. This file lives in a container volume at /root/.node-red/flows/. This means that manual changes to the flow in Node-RED instantly changes the flows file on the host.

Committing the modified flows file will cause the container to be rebuilt by Docker Hub. This seems like a good idea (for now).
