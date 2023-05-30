

# Developer Guide: Installation of a Docker Container and Deployment of an App

- [Developer Guide: Installation of a Docker Container and Deployment of an App](#developer-guide-installation-of-a-docker-container-and-deployment-of-an-app)
  - [Getting started](#getting-started)
    - [Preconditions](#preconditions)
  - [Creating a directory structure for the deployment](#creating-a-directory-structure-for-the-deployment)
  - [Deploying the Application](#deploying-the-application)
  - [Setting up the environment for docker](#setting-up-the-environment-for-docker)
  - [Creating the Dockerfile](#creating-the-dockerfile)
  - [Building the Docker Image](#building-the-docker-image)
  - [Deploying the App to the Repository](#deploying-the-app-to-the-repository)



## Getting started

This guide will walk you through deploying your application as a Docker Container in a Kubernetes cluster. Following these step-by-step instructions, you can efficiently set up your ExampleApp to accept requests on port 8800.

### Preconditions

Before proceeding with the deployment process, please ensure you have Docker installed on your machine.

If you haven't installed Docker yet, please follow the official documentation for instructions on installing it on your specific operating system.

## Creating a directory structure for the deployment

To ensure a structured deployment of your Docker container, follow these instructions and execute the commands respectively in your terminal to create the necessary directories:

1. Create the root directory for your Docker setup:
   <br>
   - `mkdir quickstart_docker`
<br>

2. Create the "application" directory within the root directory to hold your application code:
   <br>
   - `mkdir quickstart_docker/application`
<br>

3. Create the "docker" directory within the root directory. This directory will contain the Docker-related files:
<br>
   -  `mkdir quickstart_docker/docker`
<br>
   

4. Create the "application" directory within the "docker" directory. This directory will be used to store any specific files related to the Docker configuration for your application:
 <br>
   -  `mkdir quickstart_docker/docker/application`

<br>


The structure of the directories should look like this:

```python
quickstart_docker/ # Directory for the entire project
├──application/ # Directory for the application code
└──docker/ # Directory for Docker system files
└──application/ # Directory for the Dockerfile for our application
```

The "**application**" directory within the root directory holds your application code, while the "**docker**" directory contains the Docker-related files.

> **Note:** A well-organized structure for your Docker deployment helps differentiate between your application code and Docker configuration, enhancing clarity and streamlining future project work.

## Deploying the Application

To deploy an app on a Docker container, we will use the `application.py` file as an example.

Follow the steps below:

1. Copy the file `application.py` to the directory `quickstart_docker/application`.

2. In the terminal, execute the following code:

   ```python
   # Import necessary modules from the Python standard library.
   import http.server
   import socketserver

   # Set the variable PORT to 8000, which specifies the port 
   # number on which the server will listen for incoming requests.
   PORT = 8000

   # Assign the SimpleHTTPRequestHandler class from the http.server module to the variable Handler. 
   # This class provides a simple request handler that serves files and directory listings.
   Handler = http.server.SimpleHTTPRequestHandler

   # Create an instance of the TCPServer class from the socketserver module. 
   # It binds the server to listen on the specified PORT and uses the Handler class 
   # to handle incoming requests.
   httpd = socketserver.TCPServer(("", PORT), Handler)

   # Prints a message indicating that the server is serving at the specified port.
   print("Serving at port", PORT)

   # This line starts the server and makes it listen for incoming requests indefinitely.
   httpd.serve_forever()
   ```

Running this code will start a web server that listens on port 8000 and serves the requested content.


## Setting up the environment for docker

For our application to function, it needs a runtime environment specific to the programming language it was written in. In our case, `exampleapp.py` is written in Python. Hence, we will set up the environment using Python.

Download the necessary resources from Docker Hub and set up a working environment for your app with the required operating system and dependencies.

>Technical writers note: _In the context of the manual, it is recommended to provide more specific instructions or guidance on the required dependencies or resources that need to be downloaded. This will help users understand what they need to install or include to create the appropriate environment for their application._


## Creating the Dockerfile

To start setting up the environment for our app, we need to create a Dockerfile in the `quickstart_docker/docker/application` directory. The Dockerfile is a text document that contains all the commands required to assemble an image.

Follow these steps:

1. Navigate to `quickstart_docker/docker/application`.
2. Create a Dockerfile with the following content:

   ```dockerfile
   # Use base image from the registry
   FROM python:3.5

   # Set the working directory to /app
   WORKDIR /app

   # Copy the 'application' directory contents into the container at /app
   COPY ./application /app

   # Make port 8000 available to the world outside this container
   EXPOSE 8000

   # Execute 'python /app/application.py' when the container launches
   CMD ["python", "/app/application.py"]
   ```

## Building the Docker Image

To build a Docker Image, execute the command that specifies the directory, the path to the Dockerfile, and a value for tagging the created image.

In the terminal, execute the following command:

```python
docker build . -f docker/application/Dockerfile -t exampleapp
```

The arguments used in the command are as follows:

- `.`: The build context, which is the current directory.
- `-f docker/application/Dockerfile`: Specifies the path to the Dockerfile.
- `-t exampleapp`: Tags the built image as "exampleapp".

>Note: You can learn more about building Docker images from the [official Docker documentation](https://docs.docker.com/engine/reference/builder/)

To preview the built images in the terminal, execute the following command:

```python
docker images
```

You should see output similar to this:

```python
REPOSITORY             TAG         IMAGE ID       CREATED        SIZE
exampleapp             latest      83wse0edc28a   2 seconds ago  153MB
python                 3.6         05sob8636w3f   6 weeks ago    153MB
```

Description of the parameters:

- REPOSITORY: The name of the repository where the image is stored.
- TAG: The tag assigned to the image (e.g., latest).
- IMAGE ID: The unique identifier of the image.
- CREATED: The time when the image was created.
- SIZE: The size of the image.

## Deploying the App to the Repository

The final step is to push the image to the repository. Please follow the repository-specific instructions to complete this step.