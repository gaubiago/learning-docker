# My Notes on The Ultimate Docker Course

My certificate of completion is available [here](https://github.com/gaubiago/learning-docker/blob/main/certificate-of-completion-for-the-ultimate-docker-course.pdf).

## Introduction to Docker

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
    * . (dot): path to Dockerfile

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

## The Linux Command Line

* Pulling (shortcut) and start a container in interactive mode:
  ```sh
  docker run -it <image_name>
  ```
  * Land on shell logged as a user
  * The machine name is a hexa number
  * `/` is the root directory
  * `#` means you are give the highest privileges&mdash;`$` shows up for any user other than __*root*__ 

* Seeing list of running and stopped (because of `-a`) containers :
  ```sh
  docker ps -a
  ```

* Finding the location of the Shell program:
  ```sh
  echo $0
  ```

* `!<history_output_line_number>` runs the command listed in that given line of a history output

* `apt`: short for Advanced Package Tool

* Checking what packages are in the package database and which ones are installed:
  ```sh
  apt list
  ```

* Linux File System
  * `etc`: Editable Text Configuration folder stores configuration files
  * `var`: folder in which files are updated frequently
  * `proc`: includes files that represent running processes

* `ctrl+w`: remove words from the command line

* `less` command is supposed to replace `more`; `less` allows for scrolling down and up

* Redirection operators: `>` for stdout and `<` stdin
  * `>>` append to stdout

* Searching for a word in all files of a directory in a case-insensitive and recursive manner:
  ```sh
  grep -ir <word> <directory>
  ```

* Listing directories recursively:
  ```sh
  find -type d
  ```

* Listing files recursively:
  ```sh
  find -type f
  ```

* Find files that follows a certain pattern (case-insensitive):
  ```sh
  find -type f -iname "<pattern>"
  ```

* Chaining commands
  * `;`: execute commands individually (one command error does not stop other commands from executing)
  * `&&`: stop sequential execution of commands in case one command fails
  * `||`: execute one command or the other
  * `|`: what comes out of command to the left of the pipe character goes into the second command to the right of the pipe
  * `:\`: break up a long command into multiple lines

* Seeing all the environment variables in your machine:
  ```sh
  printenv
  ```

* `PATH`: variable that Linux or Windows searches for to find a program or a command

* Viewing the value of a certain environment variable
  ```sh
  printenv PATH
  ```
  or
  ```sh
  echo $PATH
  ```

* Adding new variable to the set of environment variables
  * To exist as long as the terminal session:
    ```sh
    export <VARIABLE>=<value>
    ```
  * To exist permanently:
    ```sh
    echo <VARIABLE>=<value> >> .bashrc
    ```
      * The new environment variable would be available only either on a new terminal session or after running:
        ```sh
        source ~/.bashrc
        ```

* Adding new varia

* Starting a container:
  ```sh
  docker start -i <first_chars_of_container_id>
  ```

* `TTY`: teletype   

* `pts/0`: pseudo-terminal of terminal window 0

* `&`: run command in the background (e.g. `sleep 100 &`)

* Killing a process
  ```sh
  kill <pid>
  ```
* Adding a user:
  ```sh
  useradd -m <username>
  ```
    * A similar command&mdash;more interactive and uses `useradd` under the hood:
      ```sh
      adduser <username>
      ```
  * New created users are added to `/etc/passwd`&mdash;no password is stored here
    * Passwords are stored in encrypted format in `/etc/shadow/`

* Setting the shell for a user
  ```sh
  usermod -s <path_to_shell_bin_file> <username>

* Executing a bash session on a running container and logging in with a specific username:
  ```sh
  docker exec -it -u <username> <container_id> bash
  ```
* Adding a new group:
  ```sh
  groupadd <group_name>
  ```
    * Groups are stored in `/etc/group`

* Every Linux user has one primary group and zero or more supplementary groups
  * The primary group is the group that is going to be associated with a new file created by the user if the user is a member of multiple groups
  * __Primary group__: automatically created when you create a new user&mdash;the group gets the same name as the user

* Adding a user to a supplementary group:
  ```sh
  usermod -G <group_name> <username>
  ```

* Searching for the user in `/etc/passwd`
  ```sh
  cat /etc/passwd | grep <username>
  ```
    * Output:
      ```sh
      <username>:x:<user_id>:<group_id>::<list_of_directories>
      ```

* Seeing what groups a user is part of:
  ```sh
  groups <username>
  ```

* File permissions:
  * `d` for directory and `-` for files
  * By default, all folders have `x` (execute) permission
  * Left to right:
    * 1st trio: __u__ ser
    * 2nd trio: __g__ roup
    * 3rd trio: __o__ thers
  * Adding permissions to file (e.g.):
    ```sh
    chmod uo+xw-r <filename_1> <filename_2>
    ``` 
  
## Building Images

* Image
  * A cut-down OS
  * Third-party libraries
  * Application files
  * Environment variables

* Container
  * Provides an isolated environment
  * Can be stopped and restarted
  * Is just a process

* Steps for running a web application
  * Install NodeJS
  * `npm install`: install third-party dependencies
  * `npm start`: start the web application

* Dockerfile instructions
  * `FROM`: for specifying the base image
  * `WORKDIR`: for specifying the working directory
  * `COPY` and `ADD`: for copying files and directories
    * Copying files with whitespace:
      ```sh
      # COPY [src, dst]
      COPY ["hello world.txt", "."] 
      ```
    * `ADD` can copy file from URL, and uncompressed zip files upon copying
  * `RUN`: for executing operating system commands
  * `ENV`: for setting environment variables
  * `EXPOSE`: for telling docker a container is starting on a giving port
  * `USER`: for specifying the user that should run the application&mdash; typically a user with limited privileges
  * `CMD` and `ENTRYPOINT`: for specifying the command that should be executed when starting a container

* Avoid `:latest` when using `FROM`

* Running a command when starting a container:
  ```sh
  docker run -it <container_name> <command>
  ```
  * Alpine does not come with `bash`, only with `sh`

* `.dockerignore`: used for listing files and that should be excluded

* By default, Docker runs our application with the root user, which has the highest privileges. This can open security holes in our systems. To run an application we should create a regular user with limited privileges.
  * ```sh
    # Create a group
    addgroup <group_name>
    # primary_group = group_name 
    adduser -S -G <primary_group> <system_user>
    # Verify the user is in the right group
    groups app
    ```

* When you have multiple `CMD` instructions in your Dockerfile, only the last one will take effect 

* `ENTRYPOINT` is  harder than `CMD` to override, so use `ENTRYPOINT` when you for sure want a program or command to be executed upon starting a container

* Speeding up builds: if Docker notices that an image layer has been modified at a certain point of the build, Docker has to re-run every command as of that point because of its inability to reuse the respective layers from its cache. Solution (e.g.):
  ```sh
  COPY package*.json
  RUN npm install
  COPY npm install
  ```
  * Take-away: your Docker file should have instructions or files that don't change frequently on the top, and instructions or files that change frequently should be on the bottom

* Deleting dangling images:
  ```sh
  docker ps -a
  docker container prune
  docker image prune
  ```

* Deleting an image:
  ```sh
  # It also works if the Docker image ID is provided
  docker image rm <docker_image_name>
  ```

* Tagging images:
  * If you don't tag you image properly, `latest` can point to an older image
  * `latest` is fine for development, but should not be used in production
  * While building:
    ```sh
    docker build -t <image_name>:<tag> <directory_to_be_copied>
    ```
  * After building:
    ```sh
    docker image tag <image_name>:<current_tag> <image_name>:<new_tag>
    ```

* Logging in to Docker
  ```sh
  docker login
  ```

* Pushing your image to Docker registry:
  ```sh
  docker push <your_docker_username>/<app_name>:<tag>
  ```

* Saving an image into a compressed file:
  ```sh
  docker image save -o <file_name>.tar <image_name>:<tag>
  ```

* Loading an image from a compressed file:
  ```sh
  docker image load -i <file_name>.tar
  ```

* Dockerfile sample:
  ```sh
  FROM node:14.16.0-alpine3.13
  RUN addgroup app && adduser -S -G app app
  USER app
  WORKDIR /app
  COPY package*.json .
  RUN npm install
  COPY . .
  # ENV old version: API_URL http://api.myapp.com/ 
  ENV API_URL=http://api.myapp.com/
  # Indicates what port this container will be listening on
  EXPOSE 3000
  # Shell form (CMD npm start), which spins up a new shell process (/bin/sh)
  # Execute form: makes it easier and faster to clean up resources when starting containers
  CMD ["npm", "start"] 
  ```

## Working with Containers

* Running container in the detached more (in the background):
  ```sh
  docker run -d <image_name>
  ```

* Giving a container a name when starting them:
  ```sh
  docker run -d --name <container_name> <image_name>
  ```

* Looking at the logs of a container:
  ```sh
  docker logs <container_id>
  ```
  * -f, --follow: Follow log output
  * -n, --tail string: Number of lines to show from the end of the log output
  * -t: Show timestamps

* Publishing ports:
  ```sh
  docker run -d -p <host_port>:<container_port> --name <container_name> <image_name>
  ```

* Executing commands in a running container:
  ```sh
  docker exec -it <container_name> <shell>
  ```

* Stopping and staring containers:
  ```sh
  docker stop <container_name>
  docker start <container_name>
  ```
  * `docker run` starts a new container; `docker start` starts a stop container

* Removing containers:
  ```sh
  docker stop <container_name>
  docker rm <container_name>
  ```
  or
  ```sh
  docker rm -f <container_name>
  ```
  * Pruning containers after that is a good practice:
    ```sh
    docker container prune
    ```

* Persisting data using volumes:
  * A volume is a storage outside of a container (it can be directory in the host or somewhere in the cloud)
  * Creating a volume:
    ```sh
    docker volume create <volume_name>
    ```
  * Inspecting a volume:
    ```sh
    docker volume inspect <volume_name>
    ```
  * Mapping a volume to a folder inside a container:
    ```sh
    docker run -d -p <host_port>:<container_port> -v <volume_name>:<container_folder> <image_name>
    ```
      * `volume_name` and `container_folder` are created by Docker if they don't exist. If docker create `container_folder`, you may have permission issues&mdash;if you are working with a user with limited privileges within your container.

* Copying Files between the host and the containers:
  ```sh
  docker cp <container_name>:<directory_with_filename> <host_directory>
  docker cp <host_directory_with_filename> <container_name>:<directory>
  ```

* Sharing the source code with container:
  ```sh
  docker run -d -p <host_port>:<container_port> -v <source_code_directory>:<container_directory> <image_name>
  ```
  * In addition to mapping the source-code directory in the host to the source-code directory in the container, you can also map a Docker-named volume to a folder in the container

* Dockerfile sample:
  ```sh
  FROM node:14.16.0-alpine3.13
  RUN addgroup app && adduser -S -G app app
  USER app
  WORKDIR /app
  RUN mkdir data
  COPY package*.json .
  RUN npm install
  COPY . .
  # ENV old version: API_URL http://api.myapp.com/ 
  ENV API_URL=http://api.myapp.com/
  # Indicates what port this container will be listening on
  EXPOSE 3000
  # Shell form (CMD npm start), which spins up a new shell process (/bin/sh)
  # Execute form: makes it easier and faster to clean up resources when starting containers
  CMD ["npm", "start"]
  ```

## Running Multi-container Applications

* Cleaning Up our Workspace:
  ```sh
  docker container rm -f $(docker container ls -aq)
  docker image rm -f $(docker image ls -q)
  ```

* YAML is used for configuration files, and JSON is used for exchanging files between multiple computers

* Building images:
  * `docker-compose` is built on top of Docker Engine, so it inherits commands from `docker`&mdash;the difference is that any of `docker-compose` commands will apply to the application as a whole, so most of these commands will impact multiple services or multiple containers in our application
    * `docker-compose build` options:
      * `--no-cache`: do not use cache when building the image
      * `--pull`: always attempt to pull a newer version of the image

  * When building images with compose, the `services` names are prefixed by the name of the root directory name (app's name)

* Starting and stopping the application:
  * `docker-compose up` options:
    * `--build`: build images before starting containers
    * `-d`: detached more

  * `docker-compose down`: stop and remove containers, but the images are still there

* Docker Networking:
  * Every Docker network has 3 networks
    * bridge
    * host
    * none
  * Setting the user while opening a shell to a container for pinging a container (service) in the network:
    * ```sh
      docker exec -it -u <container_name> <shell_name>
      # service_name: any of the services listed in docker-compose
      ping <service_name>
      ```
  * Knowing the ip address of the running container
    * ```sh
      ifconfig
      ```

* Viewing logs:
  * `docker-compose logs` options:
    * `-f`: follow log output
    * `t`: show timestamps
  * Seeing logs of a specific container:
    * ```sh
      docker-compose logs <container_id> -f
      ```

* Publishing changes:
  * If you have dependencies that would be installed only in the container for which you are mapping folders, you must install those dependencies on your host machine, too
  * To publish from a host to the a container, insert the following in the `services` name associated with the intended container:
    ```yml
    volumes:
      # No need for specifying an absolute directory in the host with 
      # docker-compose (as opposed to using the "docker" command)
      - ./backend:/app
    ```

* Migrating the database:
  * `migrate-mongo`
  * Remember to delete the volume being mapped
  * Overriding _Dockerfile_'s `CMD` by using _docker-compose.yml_'s `command`
    * _docker-entrypoint.sh_
      ```sh
      #!/bin/sh

      echo "Waiting for MongoDB to start..."
      ./wait-for db:27017 

      echo "Migrating the databse..."
      npm run db:up 

      echo "Starting the server..."
      npm start 
      ```

* Running tests:
  * Note: testing in a container is slower than testing in the host
  * For creating a container specifically for testing a tier of your app, add the following under `services` in _docker-compose.yml_, after building the image of the tier to be tested:
    ```sh
    <tier_name>_test:
      # Reutilize the tier image just created
      image: <app_name>_<tier_name>
      volumes:
        - ./<tier_folder>:/app
      command: npm test
    ```

## Deploying Applications

* Deployment Options
  * Single-host deployment
  * Cluster deployment: high-availability, high-scalability
  * Cluster solutions&mdash;orchestration tools:
    * Docker Swarm
    * Kubernetes

* Getting a Virtual Private Server (VPS):
  * Digital Ocean
  * Google Cloud Platform (GCP)
  * Microsoft Azure
  * Amazon Web Services (AWS)

* Installing Docker Machine
  * We need need to use [Docker Machine](https://github.com/docker/machine/releases) in the development server to talk to the Docker Engine in the production server
  * Deprecated and does not support arm64 architecture


* Provisioning a host
  * [Use Docker Machine to provision hosts on cloud providers](https://docker-docs.netlify.app/machine/get-started-cloud/)
    > Docker Machine still works as described here, but Docker Cloud supercedes Machine for this purpose.
  * Depends on the cloud platform. For DigitalOcean:
    ```sh
    docker-machine create \
    --driver digitalocean \
    --digitalocean-access-token <token> \
    <app_name>
    ```
      * `--engine-install-url`: install a specific Docker version
      * `--digitalocean-image`: specify OS image to use

* Connecting to the host
  * Finding the production application in the cloud:
    ```sh
    docker-machine ls
    ```
  * Finding out the environment variables of your application:
    ```sh
    docker-machine env <app_name>
    ```
  * Connecting to the host:
    ```sh
    eval $(docker-machine env <app_name>)
    ```
    * From now on, every command executed in the client of your machine will be sent to the Docker Engine of the target, production machine&mdash;meaning you would run `docker-compose up` to spin up your application

* Redefining the production configuration (seen below)

* Reducing the Image Size
  * ```sh
    npm run build
    ```
    * Optimizes the assets for production
    * Optimized assets are found in a new `build` folder created 
  * _Dockerbuild.prod_ sample:
    ```Dockerfile
    # Step 1: Build stage
    FROM node:14.16.0-alpine3.13 AS build-stage
    WORKDIR /app
    COPY package*.json ./
    RUN npm install
    COPY . .
    RUN npm run build

    # Step 2: Production stage
    FROM nginx:1.22-alpine
    # RUN addgroup app && adduser -S -G app app
    ##### ATTENTION: nginx requires root user to run #####
    # USER app
    # Reference previous image with the label build-stage
    COPY --from=build-stage /app/build /usr/share/nginx/html
    # Default port for web traffic
    EXPOSE 80 
    # Every time this container starts, the following command must be executed
    ENTRYPOINT ["nginx", "-g", "daemon off;"]
    ```
      * Although it is better to always set up and use a non-`root` user when building images, Nginx requires the `root` user
  * Building frontend optimized version by specifying the _Dockerfile.prod_ file:
    ```sh
    docker build -t vidly_web_opt -f Dockerfile.prod .
    ```
  * Changing _docker-compose.yml_ to target _Dockerbuild.pro_ and map host port to the correct frontend port exposed:
    ```yml
    # Should use latest version of compose (version must be specified with double quotes--string)
    version: "3.8"

    # Names under "services" are arbitrary (they could be front, backend, and database)
    services:
      web:
        build: 
          context: ./frontend
          dockerfile: Dockerfile.prod
        ports:
          # Array or list syntax
          - 80:80
        volumes:
          - ./frontend:/app
        restart: unless-stopped
      api:
        build: ./backend
        ports:
          - 3001:3001
        environment:
          # connection string: <driver>://<hostname>/<database>
          # List sytanx: - DB_URL=mongodb://db/vidly/
          # Object, or property-value syntax:
          DB_URL: mongodb://db/vidly
        command: ./docker-entrypoint.sh
        restart: unless-stopped
      db:
        # Instead of building an image, were are going to pull and image fomr Docker Hub
        # Mongo built on top of Ubuntu 16
        image: mongo:4.0-xenial
        ports:
          - 27018:27017
        volumes:
          # /data/db is the default place MongoDB stores data
          - vidly:/data/db
        restart: unless-stopped

    # Declaring the volume being used for db
    volumes:
      vidly:
    ```

  * Building all the app's images:
    ```sh
    docker-compose -f docker-compose.prod.yml build
    ```

* Deploying the application:
  * Previous versions of Docker didn't allow creating the directory `/app` implicitly with user `app` as follows&mdash;because anything created in the `root` directory needs `root` user's permissions:
    ```Dockerfile
    ...
    RUN addgroup app && adduser -S -G app app
    USER app

    WORKDIR /app
    ...
    ```
    In that case, here is the workaround:
    ```Dockerfile
    ...
    RUN addgroup app && adduser -S -G app app
    # Explicitly change the ownership (user and group) of the folder `/app`
    RUN mkdir /app && chown app:app /app
    USER app

    WORKDIR /app
    ...
    ```
  * Spinning up containers with _docker-compose.prod.yml_:
    ```yml
    docker-compose -f docker-compose.prod.yml up -d --build
    ```
  * Making sure the correct API URL for production:
    * In `frontend/src/services/api.js`, if an env. variable is not properly set up for production, it will default to the development API URL (e.g.):
      ```js
      const baseUrl = process.env.REACT_APP_API_URL || "http://localhost:3001/api";
      ```
    * The environment variable needs to be set up before running `npm run build` in _Dockerfile.prod_ in inside frontend folder:
      * Final frontend _Dockerfile.prod_: 
        ```Dockerfile
        # Step 1: Build stage
        FROM node:14.16.0-alpine3.13 AS build-stage
        WORKDIR /app
        COPY package*.json ./
        RUN npm install
        COPY . .
        ENV REACT_APP_API_URL=http://<production_hostname>:3001/api
        RUN npm run build

        # Step 2: Production stage
        FROM nginx:1.22-alpine
        # RUN addgroup app && adduser -S -G app app
        ##### ATTENTION: nginx requires root user to run #####
        # USER app
        # Reference previous image with lable build-stage
        COPY --from=build-stage /app/build /usr/share/nginx/html
        # Default port for web traffic
        EXPOSE 80 
        # Every time this container starts, the following command must be executed
        ENTRYPOINT ["nginx", "-g", "daemon off;"]
        ```

* Properly tagging your images in _docker-compose.prod.yml_ while pushing changes:
  * Final _docker-compose.prod.yml_: