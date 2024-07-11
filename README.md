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

  Below is the mongo-express response from the browser

  ![image](https://github.com/Dhanraj-d/simple-node-js-react-npm-app/assets/93528725/eaef46ab-616e-4e1b-be5d-e07490b3e695)



Q3.Setup a K3s cluster (either on AWS or on a local server) with one node to run the following pods that are connected together - fleet_manager, postgresql, Nginx, Redis,
  MongoDB, Grafana, and local docker registry. The script should install the K3s cluster, bring-up the pods and connect the two pods together (MongoDB and Mongo-express 
  with the above mentioned access credentials). Write a simple service to route externally the nginx pod to localhost through the 80 port and display the “welcome to 
  nginx” page.

ans: Created k3s cluster with one worker node on aws below are snapshots of the cluster.

  kubectl cluster-info
        
 ![Screenshot 2024-07-11 154923](https://github.com/Dhanraj-d/simple-node-js-react-npm-app/assets/93528725/ba2b1c94-def8-4970-b509-680a1e95c1d5)


  kubectl get nodes
       
![Screenshot 2024-07-11 155138](https://github.com/Dhanraj-d/simple-node-js-react-npm-app/assets/93528725/0960eabe-1386-4ef0-9337-496f8d195417)
Then created manifest files for mongodb and mongo-express below are the manifest files directory
  simple-node-js-react-npm-app/mongo-express-mongodb-files/
and deployed it on cluster.

and created nginx manifest files and deployed it on cluster below is the manifest file


          apiVersion: v1
          kind: Service
          metadata:
            name: nginx-project
          spec:
            type: NodePort
            ports:
              - port: 80
            selector:
              app: nginx-project
          ---
          apiVersion: apps/v1
          kind: Deployment
          metadata:
            name: nginx-project
          spec:
            replicas: 4
            selector:
              matchLabels:
                app: nginx-project
            template:
              metadata:
                labels:
                  app: nginx-project
              spec:
                containers:
                  - name: nginx
                    image: nginx:1.17.3
                    ports:
                      - containerPort: 80


  And exposed it on the browser below is the snapshot

  ![image](https://github.com/Dhanraj-d/simple-node-js-react-npm-app/assets/93528725/418dee47-0b02-4af1-973c-8d9702539a9b)


      





