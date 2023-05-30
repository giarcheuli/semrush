
# Developer Guide: How to set up a Docker container and deploy an application using Kubernetes


- [Developer Guide: How to set up a Docker container and deploy an application using Kubernetes](#developer-guide-how-to-set-up-a-docker-container-and-deploy-an-application-using-kubernetes)
  - [Getting started](#getting-started)
    - [Preconditions](#preconditions)
  - [Creating a directory structure for the deployment](#creating-a-directory-structure-for-the-deployment)
  - [Deploying the Application](#deploying-the-application)
  - [Setting up the environment for docker](#setting-up-the-environment-for-docker)
    - [Creating the Dockerfile](#creating-the-dockerfile)
    - [Building the Docker Image](#building-the-docker-image)
  - [Deploying the App to the Repository](#deploying-the-app-to-the-repository)
____
## Getting started

Get started with Docker container deployment using this guide. Learn to set up the directory structure, configure the Dockerfile, build the image, and deploy your application effortlessly.

### Preconditions

Ensure you have Docker installed on your machine.

If you haven't installed Docker yet, please follow the official [docker documentation](https://docs.docker.com/desktop/) for instructions on installing it on your specific operating system.
___

## Creating a directory structure for the deployment

Create a directory structure to store the project files and ensure a well-organized deployment.

Follow these steps and execute the commands respectively in your terminal to create the necessary directories:

1. Create the root directory for your Docker setup:

   - `mkdir quickstart_docker`
>

2. Create the "application" directory within the root directory to hold your application code:

   - `mkdir quickstart_docker/application`
>

3. Create the "docker" directory within the root directory. This directory will contain the Docker-related files:

   - `mkdir quickstart_docker/docker`
>

4. Create the "application" directory within the "docker" directory. This directory will be used to store any specific files related to the Docker configuration for your application:

   - `mkdir quickstart_docker/docker/application`
>

The structure of the directories should look like this:

```python
quickstart_docker/ # Directory for the entire project
├──application/ # Directory for the application code
└──docker/ # Directory for Docker system files
└──application/ # Directory for the Dockerfile for our application
```

The "**application**" directory within the root directory holds your application code, while the "**docker**" directory contains the Docker-related files.

> **Note:** A well-organized structure for your Docker deployment helps differentiate between your application code and Docker configuration, enhancing clarity and streamlining future project work.
___
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

Running this code will start a web-server that listens on port 8000 and serves the requested content.
___
## Setting up the environment for docker

To deploy your application as a Docker container, it is essential to properly set up the environment. This ensures that the container has all the necessary components and dependencies to run your application smoothly. This section will guide you through the process of creating a Dockerfile and building the Docker image for your application.

### Creating the Dockerfile

The Dockerfile is a crucial component that defines the configuration and setup of your Docker container. To begin setting up the environment, navigate to the `quickstart_docker/docker/application` directory and follow these steps:

Create a file named `Dockerfile` in the `quickstart_docker/docker/application` directory.

Open the `Dockerfile` and add the following content:

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

### Building the Docker Image

Once you have the Dockerfile ready, it's time to build the Docker image. Building the image involves assembling all the necessary dependencies and configurations specified in the Dockerfile. Follow these steps to build the Docker image:

1. Open your terminal and navigate to the root directory of your project.

2. Execute the following command:

```python
docker build . -f docker/application/Dockerfile -t exampleapp
```

This command tells Docker to build the image using the Dockerfile located at docker/application/Dockerfile and tag it as exampleapp.

3. Wait for the build process to complete. Once finished, you can preview the built images by running the command:

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


>**Note:** You can learn more about building Docker images from the [official Docker documentation](https://docs.docker.com/engine/reference/builder/)
___
## Deploying the App to the Repository

The final step is to push the image to the repository. Please follow the repository-specific instructions to complete this step.
