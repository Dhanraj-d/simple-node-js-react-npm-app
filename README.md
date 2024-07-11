  Backend Engineer - Assignment (Ati Motors)
  
Q1. Create a fork of the following repo (https://github.com/jenkins-docs/simple-node-js-react-npm-app) and add a Jenkins implementation of fetching the required npm packages to build the website and serve it locally. The website should be accessible on localhost through the port 3004 and jenkins server on 3000 with some annotations of the packages used to build the repo and button to execute the build process.

ans. I have created a fork for the above repo and installed Jenkins on the AWS server (beacuse I can't do on my laptop as i have an issue on my laptop) and changed default Jenkins port 8080 to 3000 and created pipeline for the (simple-node-js-react-npm-app) and deployed it on 3004 port.



Q2. In the fork created, create a pipeline to pull the latest postgres, redis and mongo-express and mongo db images and build them as a container locally. The images can be pulled from the public docker registry and hosted in a secured local docker registry (with the same credentials as below). Please connect the mongo-express frontend and mongo db through the following access credentials:
Username: ati
Password: ati1234

ans. I have created a pipeline to pull the latest postgres, redis and mongo-express and mongo db images, below is the directory to pipeline
     jenkins/jenkinsfile-for-dockerimages
     Then built them as a container on server, below are commands
     
      postgres: docker run -d -p 5432:5432 postgres:latest
      
      redis:    docker run -d -p 6379:6379 redis:latest
      
      For mongo-express and mongodb I have created a docker-compose file to connect eachother below is the docker-compose file
    
      version: '3'
      services:
        mongodb:
          image: mongo
          ports:
            - 27017:27017
          environment:
            - MONGO_INITDB_ROOT_USERNAME=ati
            - MONGO_INITDB_ROOT_PASSWORD=ati1234
      
        mongo-express:
          image: mongo-express
          ports:
            - 8081:8081
          environment:
            - ME_CONFIG_MONGODB_ADMINUSERNAME=ati
            - ME_CONFIG_MONGODB_ADMINPASSWORD=ati1234
            - ME_CONFIG_MONGODB_SERVER=mongodb
          depends_on:
            - mongodb 

