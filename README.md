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
docker run -d —name doctors-directory-server -p 5002:5002 -e DATABASE_HOST=mongo —network=doctors-directory-network tm63/doctors-directory-gql-node-js-server:initial-commit
```

* ### Launch the client container:
	- published port:3000
	- exposed port: 3000
	- container name: doctors-directory-client
	- environment variable DATABASE_HOST: mongo

```
docker run -d —name doctors-directory-client -p 3000:3000 -e REACT_APP_SERVER=doctors-directory-server —network=doctors-directory-network tm63/doctors-directory-react-gql-client:initial-commit
```

# Bonus:

The bonus guide includes a docker-compose YAML file configuration that simplifies the laborious task of individually setting up docker resources. It accomplishes this by interacting with the centralized YAML file configuration and managing the configured application services.

# Docker compose:

Docker Compose is a utility that enables the definition and execution of Docker applications consisting of multiple containers. By utilizing a YAML file, Docker Compose facilitates the configuration of application services. With a single command, Docker Compose empowers users to create and launch all services specified in the configuration.
