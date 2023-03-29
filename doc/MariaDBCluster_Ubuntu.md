# MariaDB Cluster with EXPRESSCLUSTER on Ubuntu
- This is an experimental work. This is not for a production environment.

## Index
- [Evaluation Environment](#evaluation-environment)
- [Prerequisite](#prerequisite)
- [Clustering](#clustering)
- [Add Database Monitor](#add-database-monitor)

## Evaluation Environment
```
  +------------------------+
  | server1                |   +-------------+
  | - Ubuntu Server        |   |             |
  | - Docker               +---+ Mirror Disk |
  | - EXPRESSCLUSTER X 5.0 |   |             |
  +------------------------+   +------|------+
                                      | Mirroring
  +------------------------+   +------V------+
  | server2                |   |             |
  | - Ubuntu Server        +---+ Mirror Disk |
  | - Docker               |   |             |
  | - EXPRESSCLUSTER X 5.0 |   +-------------+
  +------------------------+
```
- Ubuntu 22.04.2 LTS
  ```
  $ uname -r
  5.15.0-69-generic
  ```
  ```
  cat /etc/lsb-release
  DISTRIB_ID=Ubuntu
  DISTRIB_RELEASE=22.04
  DISTRIB_CODENAME=jammy
  DISTRIB_DESCRIPTION="Ubuntu 22.04.2 LTS"
  ```
- Docker
  ```
  $ docker --version
  Docker version 20.10.21, build 20.10.21-0ubuntu1~22.04.2
  ```
- EXPRESSCLUSTER X 5.0 (5.0.2-1)

## Prerequisite
- Install EXPRESSSCLUSTER and create a cluster. For details, refer to [Installation and Configuration Guide](https://docs.nec.co.jp/sites/default/files/minisite/static/1639a2de-5285-471a-817b-d0b98603d987/ecx_x50_linux_en/index.html).
- Add the following resources.
  - Mirror Disk Resource
    - Mount Point: /mnt/md1
  - Floating IP Resource
- Install docker and mariadb-client on both nodes.
  ```sh
  sudo apt install docker.io mariadb-client -y
  ```
  - See also [How to Install Docker](./HowToInstallDocker.md).

## Clustering
1. Start Cluster WebUI and add Exec Resource.
1. Edit start.bat and stop.bat as below.
   - [start.sh](../script/MariaDB/start.sh)
   - [stop.sh](../script/MariaDB/stop.sh)
1. Recommend to enable script log.
   - Resource Properties
     1. [Details] tab
     1. [Tuning]
     1. [Maintenance] tab
        - Log Output Path: /opt/nec/clusterpro/log/exec-mariadb1
        - Rotate Log: Check
1. Click [Apply the Configuration File].
1. Start the failover group.
1. Check the cluster status.

## Add Database Monitor
1. On Cluster WebUI, add MySQL Monitor Resource as below.
   - Monitor Level: Level 2
   - Database Name: watch
   - IP Address: 127.0.0.1
   - Port: 3306
   - User Name: root
   - Password: password
   - Table: mysqlwatch
   - Storage Engine: InnoDB
   - Library Path: /usr/lib64/libmariadb.so.3
1. Click [Apply the Configuration File].
1. Check the cluster status.