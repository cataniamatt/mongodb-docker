# MongoDB Step-by-Step
This is a guide for creating a simple MongoDB database with some collections and documents. In MongoDB, a collection is similar to a table in SQL while documents are similar to rows in tables.

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
docker-compose up -d
```
```
Creating network "mongodb_network" with the default driver
Creating volume "mongodb-docker_data" with default driver
Creating mongodb       ... done
Creating mongo-express ... done
```
The '-d' tag is used to run all the containers in the detached mode, meaning in the background.

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





Start an interactive shell for the mongodb container using the following command:
docker exec -it container_name bash
"-i" tag interactive mode
"-t" tag
 
On the mongodb container run the following command to access the database
mongo mongodb://localhost:27017 -u admin -p password

