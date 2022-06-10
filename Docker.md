# Docker
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