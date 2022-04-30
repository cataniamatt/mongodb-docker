# MongoDB Docker Container Step-by-Step
This is a guide for creating a simple MongoDB databasem with some collections and documents, inside a Docker container. In MongoDB, a collection is similar to a table in SQL while documents are similar to rows in tables.

![MongoDB Logo](https://github.com/cataniamatt/mongodb-docker/blob/main/mongodb.png)

In this guide, a MongoDB database will be created as a Docker container. Mongo Express, which is a web-based adiministrative tool to manage MongoDB databases, will also be installed as another Docker container. 

## Creating the Docker containers
The code below is from the "docker-compose.yml" file that will be used to create both containers. Both containers will use the latest images of MongoDB and Mongo Express that are found on Docker Hub. These will be downloaded from Docker Hub and used to create the containers when Docker Compose is started. 
```
services:
  mongodb:
    image: mongo
    container_name: mongodb
    ports:
      - 27017:27017
    volumes:
      - data:/data
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
  mongo-express:
    image: mongo-express
    container_name: mongo-express
    restart: always
    ports:
      - 8081:8081
    volumes:
      - data:/data
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_MONGODB_SERVER=mongodb
volumes:
  data:

networks:
  default:
    name: mongodb_network
```

Once the "docker_compose.yml" file is complete with the desired containers and configurations,the containers can be started by using the following command:
```
docker-compose up
```
The '-d' tag is used to run all the containers in the detached mode, meaning in the background.

```
Creating network "mongodb_network" with the default driver
Creating volume "mongodb-docker_data" with default driver
Creating mongodb       ... done
Creating mongo-express ... done
```

### Volumes
In this example, a named volume called "data" will be used to save the data inside the MongoDB database onto the host machine so that it is not lost if the container is deleted. 

Docker supports three types of volumes:
* Host volumes
    * The path on the host is defined by the user.
    * Syntax: /path/on/host:/path/in/container
* Anonymous volumes
    * Docker handles the storage location but its difficult to reuse by multiple containers. Only the path in the container has to be specified.
    * Syntax: /path/in/container
* Named volumes
    * Similar to anonymous volumes but can be given a name so that they can be referred to by multiple containers.
    * Syntax: volume_name:/path/in/container

If a named volume is used, the volume should be created at the services level as can be seen in the sample "docker_compose.yml" file above.

## Mongo Express
At this stage, MongoDB and Mongo Express are both running on the localhost. The MongoDB database can be accessed using Mongo Express by going to the following address in a browser:
```
localhost:8081
```
or
```
127.0.0.1:8081
```
The 8081 is the host port that was mapped to port 8081 of the container, this is the default port used by Mongo Express.

## MongoDB Shell
A MongoDB database can also be managed using the MongoDB Shell directly on the container. To do this, an interactivate shell for the MongoDB container must first be started using:
```
docker exec -it container_name bash
```
The "-t" tag starts a pseudo terminal while the "-i" tag allows for interaction with the pseudo terminal. Together they allow commands to be executed on the bash shell that is started.

Once inside the container, execute the following command to open a MongoDB Shell for the database.
```
mongo mongodb://localhost:27017 -u admin -p password
```

The MongoDB commands can now be used in the shell to interact with the database.
```
root@ff49ab8bc52e:/# mongo mongodb://localhost:27017 -u admin -p password
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	https://docs.mongodb.com/
Questions? Try the MongoDB Developer Community Forums
	https://community.mongodb.com
---
The server generated these startup warnings when booting:
        2022-04-30T16:35:23.983+00:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
---
---
        Enable MongoDB's free cloud-based monitoring service, which will then receive and display
        metrics about your deployment (disk utilization, CPU, operation statistics, etc).

        The monitoring data will be available on a MongoDB website with a unique URL accessible to you
        and anyone you share the URL with. MongoDB may use this information to make product
        improvements and to suggest MongoDB products and deployment options to you.

        To enable free monitoring, run the following command: db.enableFreeMonitoring()
        To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---
>
```

## MongoDB Shell Commands
All MongoDB shell commands can be found within the MongoDB documentation [here](https://www.mongodb.com/docs/manual/reference/mongo-shell/). Below are some of the most commonly used commands.

### Databases
To view all databases:
```
show dbs
```

To create a new database:
```
use database_name
```

To view current database being used:
```
db
```

To delete a database, switch to the database and do:
```
db.dropDatabase()
```

### Collections
To create a collection:
```
db.createCollection("collection_name")
```
OR
```
db.createCollection("collection_name", {
    capped: <boolean>,
    size: 6142800,
    max: 3000
})
```
This is used for creating a collection with a custom configuration.

To view the configuration and statistics of a collection:
```
db.collection_name.stats()
```

To view all collections:
```
show collections
```

### Documents
To insert documents into a collection, first create a variable that holds a JSON object/s in it and then use the "insert" function with it. MongoDB will automatically assign an object ID to each object. No schema is required unlike SQL, MongoDB will automatically assign the data type.
```
variable = [
    {
        "name": "John",
        "surname": "Doe",
        "age": 22       
    },
    {
        "name": "Jane",
        "surname": "Doe",
        "age": 21
    }
]

db.collection_name.insert(variable)
```

### Queries
To query all documents in a collection
```
db.collection_name.find().pretty()
```
The pretty() function is used to output the documents in a more readable format. 

To query from a database, insert the query within the find() function. A comprehensive list of all queries possible can be found in the MongoDB documentation [here](https://www.mongodb.com/docs/v4.2/reference/method/db.collection.find/).
