# How to Install Witness Server
- You don't need to install witness server by yourself because the witness server container image is available on [Docker Hub](https://hub.docker.com/repository/docker/expresscluster/clpwitnessd/general). If you want to install it by yourself, refer to the following steps.

## Install Witness Server
1. Make a work directory as below and move to the directory.
   ```sh
   mkdir clpwitnessd
   ```
   ```sh
   cd clpwitnessd
   ```
1. Copy clpwitness file (e.g., clpwitnessd-5.1.0.tgz) from EXPRESSCLUSTER CD image to the above direcotry.
1. Expand the archive file.
   ```sh
   tar xzvf clpwitnessd-5.1.0.tgz
   ```
1. Edit package/clpwitnessd.conf.js as below.
   ```js
   module.exports = {
           http: {
                   enable: true,
                   port: 8080,   
   (snip)
   ```
1. Create tgz file as below.
   ```sh
   tar czvf clpwitnessd-5.1.0.tgz package/
   ```
1. Copy [Dockerfile](../Dockerfile/clpwitnessd/Dockerfile) to the above directory.
1. Run the following command.
   ```sh
   docker build -t clpwitnessd:5.1.0 .
   ```
