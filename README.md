## Docker Node.js App with MongoDB, AWS ECR and Express

This app shows a simple user profile app set up using 
- **Frontend:** index.html with pure JavaScript and CSS styles.
- **Backend:** Node.js powered by the Express module.
- **Data Storage:** MongoDB is used for storing data.
- **AWS ECR:** Private repository for docker image.

All components are containerized using Docker.

### Using Docker

#### To start the application

Step 1: Create a docker network

    docker network create mongo-network 

Step 2: Start Mongodb 

    docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=pass --name mongo --network mongo-network mongo    

Step 3: Start Mongo Express
    
    docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=pass --network mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongo mongo-express   

_NOTE: creating docker-network is optional. You can start both containers in a default network. In this case, just emit `--network` flag in `docker run` command_

Step 4: Open Mongo Express in your browser.

    http://localhost:8081

Step 5: create `user-account` _db_ and `users` _collection_ in mongo-express

Step 6: Start your Node.js application locally - navigate to `app` directory of project 

    cd app
    npm install 
    node server.js
    
Step 7: Access your Node.js application UI in the browser

    http://localhost:3000

### Using Docker Compose

#### To start the application

Step 1: Start MongoDB and Mongo Express

    docker-compose -f docker-compose.yaml up
    
_You can access the mongo-express under localhost:8080 from your browser_
    
Step 2: In the Mongo Express UI, create a new database named "my-db"

Step 3: In the Mongo Express UI, create a new collection named "users" in the database "my-db"       
    
Step 4: Start the Node server

    cd app
    npm install
    node server.js
    
Step 5: Access the Node.js application in the browser 

    http://localhost:3000


#### To build a docker image from the application

    docker build -t node-app:1.0 .       
    
The dot "." at the end of the command denotes location of the Dockerfile.

### To push a docker image to AWS ECR (Elastic Container Registry)

Step 6: Build your Docker image

    docker build -t node-app:1.0 .

Step 7: Authenticate Docker to your AWS ECR registry.

Step 8: Tag the built Docker image

    docker tag node-app:1.0 your-account-id.dkr.ecr.your-region.amazonaws.com/node-app:1.0

Step 9: Push the Docker image to AWS ECR.

    docker push your-account-id.dkr.ecr.your-region.amazonaws.com/node-app:1.0

Now the Docker image is pushed to AWS ECR and can be used in ECS (Elastic Container Service) or any other AWS service that supports Docker containers.