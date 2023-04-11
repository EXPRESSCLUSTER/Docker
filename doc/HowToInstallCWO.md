# How to Install Cluster WebUI Offline
- You don't need to install witness server by yourself because the witness server container image is available on [Docker Hub](https://hub.docker.com/repository/docker/expresscluster/cwo/general). If you want to install it by yourself, refer to the following steps.

## Install Cluster WebUI Offline
1. Make a work directory as below and move to the directory.
   ```sh
   mkdir cwo
   ```
   ```sh
   cd cwo
   ```
1. Copy clpwitness files from EXPRESSCLUSTER CD image to the above direcotry.
   ```
   cluster-webui-lin-5.1.0-221226-1.tgz .
   cluster-webui-sss-lin-5.1.0-221226-1.tgz .
   cluster-webui-sss-win-5.1.0-221226-1.tgz .
   cluster-webui-win-5.1.0-221226-1.tgz .
   ```
1. Copy [Dockerfile](../Dockerfile/cwo/Dockerfile) to the above directory.
1. Run the following command.
   ```sh
   docker build -t cwo:5.1.0 .
   ```
