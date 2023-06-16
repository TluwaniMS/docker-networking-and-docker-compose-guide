# Docker networking and Docker compose guide.

The primary purpose of this guide is to provide a step-by-step tutorial on how to launch a full stack application, which includes a user interface, a server, and a database, utilizing docker containers. The essential takeaway from this guide is to offer a practical understanding of container networking and communication.

### Practice guide:

* ### Create the network:
	- driver: bridge
	- network-name: doctors-directory-network

```
docker network create -d bridge doctors-directory-network
```

* ### Launch the mongodb container:
	- published port: 27017
	- exposed port: 27017
	- container name: mongo

```
docker run -d -p 27017:27017 --name mongo --network=doctors-directory-network mongo
```

* ### Launch the server container:
	- published port: 5002
	- exposed port: 5002
	- container name: doctors-directory-server
	- environment variable DATABASE_HOST: mongo

```
docker run -d --name doctors-directory-server -p 5002:5002 -e DATABASE_HOST=mongo --network=doctors-directory-network tm63/doctors-directory-gql-node-js-server:migration-script-int-commit
```

* ### Launch the client container:
	- published port:3000
	- exposed port: 3000
	- container name: doctors-directory-client

```
docker run -d --name doctors-directory-client -p 3000:3000 --network=doctors-directory-network tm63/doctors-directory-react-gql-client:env-config-update-commit
```

# Bonus:

The bonus guide includes a docker-compose YAML file configuration that simplifies the laborious task of individually setting up docker resources. It accomplishes this by interacting with the centralized YAML file configuration and managing the configured application services.

# Docker compose:

Docker Compose is a utility that enables the definition and execution of Docker applications consisting of multiple containers. By utilizing a YAML file, Docker Compose facilitates the configuration of application services. With a single command, Docker Compose empowers users to create and launch all services specified in the configuration.

# Docker compose file guide:

* ### service:

A `Service` refers to an abstract representation of a computing resource in an application that can be independently scaled or replaced without affecting other components. `Services` are supported by a collection of containers that are managed by the platform, considering replication needs and placement restrictions.

* ### container_name:

`container_name` represents a string that allows the specification of a personalized name for a container, as opposed to using an automatically generated default name.

* ### depends_on:

`depends_on` is used to indicate the dependencies between services during startup and shutdown processes.

* ### environment:

The `environment` in the container defines the `environment` variables that are configured. This can be done using either an array or a map.

array approach:

```
environment:
  RACK_ENV: development
  SHOW: "true"
  USER_INPUT:
```

map approach:

```
environment:
  - RACK_ENV=development
  - SHOW=true
  - USER_INPUT
```

* ### image:

The `image` parameter specifies the `image` from which to initiate the container.

* ### networks:

`Networks` serve as the intermediary layer facilitating communication among services, and the `networks` nested within services define the `networks` to which service containers are connected.

`e.g`

```
services:
  some-service:
    networks:
      - some-network
      - other-network
```

* ### ports:

Reveals container `ports`.

## Practice Guide:

###### Start up the fullstack application using the compose file:

```
docker compose -f doctors-directory-compose-file.yaml up -d 
```

###### Shut down the fullstcak application using the compose file:

```
docker compose -f doctors-directory-compose-file.yaml down
```
