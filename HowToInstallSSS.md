# How to Install EXPRESSCLUSTER X SingleServerSafe on Container
- This article shows how to install EXPRESSCLUSTER X SingleServerSafe on a container with Dockerfile. The SingleServerSafe container images are available on [Docker Hub](https://hub.docker.com/orgs/expresscluster). If you use the container image with kubernetes, please refer to [https://github.com/EXPRESSCLUSTER/kubernetes](https://github.com/EXPRESSCLUSTER/kubernetes). If you want to create a SingleServerSafe container image by yourself, please do the following steps.

## Index
- [Evaluation Environment](#Evaluation-Environment)
- [Prerequisite](#Prerequisite)
- [Install EXPRESSCLUSTER X SingleServerSafe with Dockerfile](#Install-EXPRESSCLUSTER-X-SingleServerSafe-with-Dockerfile)

## Evaluation Environment
- We have evaluated on the following container host environment.
  - Ubuntu 18.04.4 LTS, Docker 19.03.5

## Prerequisite
- Install Docker following the steps in [How to Install Docker](https://github.com/EXPRESSCLUSTER/Docker/blob/master/HowToInstallDocker.md).
- Download EXPRESSCLUSTER X SingleServerSafe install module.
  ```sh
  $ curl -O https://www.nec.com/en/global/prod/expresscluster/en/trial/zip/ecxsss41l_amd64.zip
  $ unzip ecxsss41l_amd64.zip
  $ cd ecxsss41l_amd64/Linux/4.1/en/server/
  $ $ ls
  expressclssss-4.1.1-1.amd64.deb
  ```

## Install EXPRESSCLUSTER X SingleServerSafe with Dockerfile
1. Download the following file.
   - [For MariaDB]()
   - [For PostgreSQL]()
1. Expand the above file.
   ```
   <Parent Directory>
    |
    +-- Dockerfile
    +-- entrypoint.sh
    +-- license/
    +-- package/
    +-- scripts/
   ```
   - The license directory contains the **trial licenses** are available until March 31, 2021.
1. Copy expressclssss-4.1.1-1.amd64.deb file to package directory.
1. Move to the directory contains Dockerfile and run the following command.
   ```sh
   $ sudo docker build --build-arg http_proxy=<proxy server>:<port number> -t <container image name (e.g. sss4mariadb:4.1.1-1.)> .
   ```
1. Check if the container image exists.
   ```sh
   $ sudo docker images
   REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZE
   sss4postgres                         4.1.2-1.1           f7a9ef43da73        2 minutes ago       260MB
minutes ago       214MB
   sss4mariadb                          4.1.1-1.1           1013ca060610        44 minutes ago      313MB
   ```

