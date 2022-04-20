# How to Install Docker
- If you use Docker with kubernetes, please refer to [kubernetes web site](https://kubernetes.io/docs/setup/production-environment/container-runtimes/.)

## Index
- [Evaluation Environment](#Evaluation-Environment)
- [Install Docker on CentOS](#Install-Docker-on-CentOS)
- [Install Docker on Ubuntu](#Install-Docker-on-Ubuntu)

## Evaluation Environment
- CentOS 7.6.1810 (3.10.0-957.21.3.el7.x86_64)
- Ubuntu 18.04.2 LTS (4.15.0-54-generic)

## Install Docker on CentOS
1. If your environment behind a proxy server, edit yum.conf.
   ```bash
   # vi /etc/yum.conf
     :
   proxy=http://<your proxy server>:<port number>
   ```
1. Check update and install.
   ```bash
   yum check-update
   yum -y update
   ```
1. Install Docker.
   ```bash
   yum -y install docker
   ```
1. Create the following directory and file.
   ```bash
   mkdir /usr/lib/systemd/system/docker.service
   touch /usr/lib/systemd/system/docker.service/http-proxy.conf
   ```
1. Edit http-proxy.conf as below.
   ```
   [Service]
   Environment="HTTP_PROXY=http://<your proxy server>:<port number>"
   Environment="HTTPS_PROXY=http://<your proxy server>:<port number>"
   ```
1. If your enviroment requires CRT file to go to the internet, please save CRT file on the following directory.
   ```
   /usr/share/pki/ca-trust-source/anchors
   ```
1. Restart Docker.
   ```bash
   systemctl daemon-reload
   systemctl restart docker
   ```
1. Enable and start Docker.
   ```bash
   systemctl enable docker
   ```

## Install Docker on Ubuntu
1. If your environment behind a proxy server, edit /etc/atp/apt.conf as below.
   ```
   Acquire::http::proxy "http://<your proxy server>:<port number>";
   Acquire::https::proxy "http://<your proxy server>:<port number>";
   ```
1. Check update and install.
   ```bash
   apt-get update
   apt-get upgrade
   ```
1. Install Docker.
   ```bash
   apt-get install docker.io
   ```
1. Make directory and create http-proxy.conf file.
   ```bash
   mkdir -p /etc/systemd/system/docker.service.d
   vi /etc/systemd/system/docker.service.d/http-proxy.conf
   ```
1. Add the following lines to http-proxy.conf.
   ```
   [Service]
   Environment="HTTP_PROXY=http://<your proxy server>:<port number>"
   Environment="HTTPS_PROXY=http://<your proxy server>:<port number>"
   ```
1. Restart Docker.
   ```bash
   systemctl daemon-reload
   systemctl restart docker
   ```
1. Enable and start Docker.
   ```bash
   systemctl enable docker
   ```
