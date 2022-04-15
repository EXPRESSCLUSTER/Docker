# How to Install Witness Server
- You don't need to install witness server by yourself because the witness server docker image is available on [Docker Hub](https://hub.docker.com/r/expresscluster/clpwitnessd). If you want to install it by yourself, refer to the following steps.

## Install Witness Server with Dockerfile 
1. Pull Node.js image.
   ```sh
   # docker pull node:latest
   ```
1. Make a work direcotry and move to the directory.
   ```sh
   # mkdir clpwitnessd
   # cd clpwitnessd
   ```
1. Copy clpwitness file (e.g. clpwitnessd-4.2.0.tgz) and save it on the work directory.
1. Copy [Dockerfile from GitHub](https://github.com/EXPRESSCLUSTER/Docker/blob/master/Dockerfile/clpwitnessd/Dockerfile) and save it on the work directory.
1. Check the following files are on the same directory.
   ```sh
   # ll
   total 8
   -rw-r--r-- 1 root root 3157 Aug  3 12:29 clpwitnessd-4.2.0.tgz
   -rw-r--r-- 1 root root  133 Aug  3 12:29 Dockerfile
   ```
1. Edit Dockerfile if you want to change the clpwitness version.
   ```
   FROM node:latest
   
   COPY clpwitnessd-4.2.0.tgz /tmp
   RUN npm install --global /tmp/clpwitnessd-4.2.0.tgz
   
   EXPOSE 80
   CMD ["clpwitnessd"]
   ```
1. Build the image.
   ```sh
   # docker build -t clpwitnessd:4.2.0 .
   ```
1. Check the container image is created.
   ```sh
   # docker images | grep clpwitness
   clpwitnessd                                     4.2.0                   23765cb3e620        14 hours ago        943 MB
   ```