# MongoDB Step-by-Step
This is a guide for creating a simple MongoDB database with some collections and documents. In MongoDB, a collection is similar to a table in SQL while documents are similar to rows in tables.


Start an interactive shell for the mongodb container using the following command:
docker exec -it container_name bash
"-i" tag interactive mode
"-t" tag
 
On the mongodb container run the following command to access the database
mongo mongodb://localhost:27017 -u admin -p password

