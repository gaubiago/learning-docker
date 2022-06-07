# Docker

* Dockerfile provides instructions for Docker to package an application into an image
* A container is a process that has its own file system, which is provided by an image
* Docker Hub is a registry for Docker images that anyone can use 
* Dockerfile: must be added at the root directory of the app to be packaged into an image. E.g.:

  ```dockerfile
  FROM node:alpine
  COPY . /app
  WORKDIR /app
  CMD node app.js
  ```

* Building the image:

  ```sh
  docker build -t <image_name> .
  ```
    * -t (--tag): used to define the image name
    * . (period): path to Dockerfile

* Listing Docker images:

  ```sh
  docker images
  ```

  or

   ```sh
  docker image ls 
  ```

* Running an image:

  ```sh
  docker run <image_name>
  ```

* Downloading an image

  ```sh
  docker pull <image_name>
  ```
