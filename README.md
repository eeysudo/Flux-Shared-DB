# Flux Shared DB

Flux Shared DB is a solution for persistent shared DB storage on the [Flux network](https://www.runonflux.io), It handles replication between various DB engine instances (MySQL, ~~MongoDB~~ and ~~PostgreSQL~~). The operator nodes discover each other using FluxOS api and immediatley form a cluster. Each Operator node is connected to a DB engine using a connection pool, recieved read queries are proxied directly to the DB engine and write queries sent to the master node. master nodes timestamp and sequence recieved write queries and immidiately forward them to the slaves.

![FLUX DB Cluster](https://user-images.githubusercontent.com/1296210/184499730-722801f7-e827-4857-902e-fe9a61f36e5f.jpg)

The Operator has 3 interfaces:
1. DB Interface (proxy interface for DB engine)
2. Internal API (used for internal communication)
3. UI API (used for managing cluster)

DB Interface is listening to port 3307 by default and acts as a proxy, so if you're using MySql as DB engine it will behave as a MySql server.

## Running it on Flux network

In order to use Flux Shared DB you need to run it as a composed docker app with at least these 2 components:
1. DB engine (MySql)
2. [Operator](https://hub.docker.com/r/alihmahdavi/fluxdb)
3. Your Application

### Options/Enviroment Parameters:
* DB_COMPONENT_NAME (required) - this is host name for db engine component, it should be provided with this format `flux[db engine component name]_[application name]`
* INIT_DB_NAME - this is the initial database name that will be created by the operator after initialization.
* DB_INIT_PASS - root password for DB engine.
* DB_PORT - external db port for DB interface, this port can be used to connect to the cluster remotely and manage the database.
* API_PORT - external api port for cluster communication.
* DB_APPNAME (required) - name of the application on Flux network.
* CLIENT_APPNAME - in case you want to give access to another application to connect, give name of the application running on Flux
* WHITELIST - comma seperated list of IP's that can connect to the DB_PORT remotely


TODO:

-- implement mongoDB support

-- implement postgreSql support

