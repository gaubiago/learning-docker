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