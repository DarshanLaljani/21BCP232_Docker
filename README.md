# 21BCP232_Docker

# Containerizing a 3 tier MERN application

This repository contains the Dockerfiles, .dockerignore file, and docker-compose.yml file for a full-stack application with a React frontend and a Node.js backend connected to a MongoDB database.


## Backend Dockerfile

The Backend Dockerfile contains the following instructions:

- `FROM node:latest`
  - Specifies the base image for the backend container as the latest version of the official Node.js Docker image.

- `WORKDIR /app`
  - Sets the working directory within the container to `/app`.

- `COPY ..`
  - Copies the contents of the `..` directory (which should contain the backend code) into the current working directory (`/app`) inside the container.

- `RUN npm install`
  - Installs all the dependencies specified in the `package.json` file for the backend application.

- `EXPOSE 5001`
  - Exposes port 5001 inside the container, allowing external access to the backend application.

- `CMD ["npm", "run", "start","--","--host","0.0.0.0"]`
  - Specifies the command to be executed when the container starts. In this case, it runs the `devStart` script defined in the `package.json` file, which likely starts the backend application in development mode.


To create the docker image, simply open up your terminal and change your present working directory to the root directory of your backend. Make sure the docker file is in the same directory and run the command
    
            docker build -t image_name .

## Frontend Dockerfile

The Frontend Dockerfile contains the following instructions:

- `FROM node:latest`
  - Specifies the base image for the frontend container as the latest version of the official Node.js Docker image.

- `WORKDIR /app`
  - Sets the working directory within the container to `/app`.

- `COPY . .`
  - Copies the entire contents of the current directory (which should contain the frontend code) into the `/app` directory inside the container.

- `RUN npm install`
  - Installs all the dependencies specified in the `package.json` file for the frontend application.

- `EXPOSE 8080`
  - Exposes port 8080 inside the container, allowing external access to the frontend application.

- `CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]`
  - Specifies the command to be executed when the container starts. In this case, it runs the `dev` script defined in the `package.json` file, which likely starts the frontend application in development mode. The `--host 0.0.0.0` option allows the frontend to be accessible from outside the container.
 

To create the docker image, simply open up your terminal and change your present working directory to the root directory of your frontend. Make sure the docker file is in the same directory and run the command
    
            docker build -t image_name .

## .dockerignore

The `.dockerignore` file specifies files and directories that should be ignored by Docker when building the image. This can help reduce the image size and improve build times by excluding unnecessary files. In this case, the following items are ignored:
Ignore node_modules directory
node_modules

Ignore npm debug logs
npm-debug.log

Ignore any .env files (containing sensitive information)
.env

Ignore any files generated during development

.DS_Store
*.log
*.pid
*.seed
*.pid.lock

Ignore any build artifacts

/build

Ignore any test coverage reports

/coverage

Ignore any editor-specific files
.idea
.vscode
*.sublime-project
*.sublime-workspace

 Ignore Docker-specific files
.dockerignore
.dockerignore.example
.dockerignore.sample
Dockerfile
docker-compose.yml



## docker-compose.yml

The `docker-compose.yml` file is used to define and configure multi-container Docker applications. It contains the following services:

- `frontend`
  - `image: darshanlaljani/21bcp232fend`
    - Specifies the Docker image to be used for the frontend container. Change it to whatever name you have given
  - `ports`
    - Maps the host port (`8080`) to the container port (`8080`), allowing access to the frontend application from outside the container.

- `backend`
  - `image: darshanlaljani/21bcp232bend`
    - Specifies the Docker image to be used for the backend container. Change it to whatever name you have given
  - `ports`
    - Maps the host port (`5001`) to the container port (`5001`), allowing access to the backend application from outside the container.
  - `depends_on`
    - Specifies that the backend container depends on the `mongodb` service, ensuring that the MongoDB container is started before the backend container.

- `mongodb`
  - `image: mongo`
    - Specifies the official MongoDB Docker image.
  - `container_name: mongodb`
    - Sets a custom name for the MongoDB container.
  - `ports`
    - Maps the host port (`27017`) to the container port (`27017`), allowing access to the MongoDB instance from outside the container.
  - `environment`
    - Sets environment variables for the MongoDB container.
    - `MONGO_INITDB_DATABASE=test`: Specifies the name of the initial database to be created.
    - `MONGO_INITDB_ROOT_USERNAME=mongo_user`: Sets the root username for the MongoDB instance.
    - `MONGO_INITDB_ROOT_PASSWORD=mongo_pass`: Sets the root password for the MongoDB instance.

By using Docker Compose, you can easily start and manage all the containers required for your application with a single command.
