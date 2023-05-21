# infrastructure-101
fiddling with infrastructure 


# problem - supercloud 

the initial test is to experiment with Supercloud. 

A supercloud refers to computing environment or platform that is designed to work seamlessly across multiple cloud services. 

You want to become independent of the clouds and you want to create solutions that are not stuck with a certain infrastructure. 

The obvious problem is that it will be complicated, and trying to find general infrastructure solutions will most likely be hard. 

We will experiment with how it would look to setup a simple solution in three cloud infratstructures to feel the first issues. 

We will implement a front-end using a back-end microservice (HelloWorld) and when it works to access the same solution on the three infrastructures. 

So how do we do this? What can we learn from it? 


# solution - first pass 

- We will use VS code, Git and Github as a start and create a repository for it. 
- we will write a microservice in Node.js that will be deployed to all the cloud platforms 
- Create a dockerfile to encapsulate the services (alternative is to use serverless - but we do a first try)
- Create a terraform configuration file (.tf) to describe the infrastructure required on each platform (AWS, GCP and Azure)
- Setup Github Actions to automate the build of the docker image and deployment of infrastructure/service (workflow.yml) - checkout code, build docker image, push image to registry, run terraform..)
- Push changes - committing microservices, dockerfile, terraformfile, github actions to the github repo - will lead GitHub actions to build and deploy service 

## Hello World microservice 

```node 
// Import the express module
const express = require('express');

// Create a new express application
const app = express();

// Define the port to listen on. Use the PORT environment variable if it's set.
const port = process.env.PORT || 3000;

// Handle GET requests to the root path
app.get('/', (req, res) => {
    res.send('Hello World');
});

// Start listening for requests
app.listen(port, () => {
    console.log(`Hello World service listening at http://localhost:${port}`);
});

```

To install the microservice Node and Express need to be setup: 

```bash
# Initialize a new Node.js project
npm init -y

# Install Express
npm install express

```
The microservice is run by "node hello-world.js" 

## Create a Docker file (to encapsulate service)

First write documentation in docker.md - then do the docker-file: 

```docker 
# Use an official Node.js runtime as the base image
FROM node:14

# Set the working directory in the container to /app
WORKDIR /app

# Copy the package.json and package-lock.json files into the container
COPY package*.json ./

# Install the application dependencies inside the container
RUN npm install

# Copy the rest of your application's source code into the container
COPY . .

# Expose port 3000 in the container
EXPOSE 3000

# Define the command to run your app using CMD which defines your runtime
CMD [ "node", "app.js" ]
```








