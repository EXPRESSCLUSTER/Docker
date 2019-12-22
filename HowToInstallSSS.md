# How to Install EXPRESSCLUSTER X SingleServerSafe on Container

## Index
- [Prerequisite](#Prerequisite)
- [Install EXPRESSCLUSTER X SingleServerSafe on CentOS Container](#Install-EXPRESSCLUSTER-X-SingleServerSafe-on-CentOS-Container)
- [Install SingleServerSafe on MariaDB Container](#Install-SingleServerSafe-on-MariaDB-Container)

## Prerequisite
- Install Docker following the steps in [How to Install Docker](https://github.com/EXPRESSCLUSTER/Docker/blob/master/HowToInstallDocker.md).
- We have evaluated on the following container host environment.
  - CentOS 7.6.1810 (3.10.0-957.21.3.el7.x86_64)
  - Docker 1.13.1
- Save EXPRESSCLUSTER rpm file and license files on a directory (e.g. /root/work) of container host.

## Install EXPRESSCLUSTER X SingleServerSafe on CentOS Container
1. Create the script file as below and save it on a directory (e.g. /root/work) of container host.
   ```bash
   # start.sh
   cd /opt/nec/clusterpro/etc/systemd
   ./clusterpro_evt.sh start
   ./clusterpro_trn.sh start
   ./clusterpro_alertsync.sh start
   ./clusterpro_webmgr.sh start
   tail -f /dev/null
   ```
   And change file mode bits as below.
   ```bash
   # chmod 755 start.sh
   ```
1. Pull a container image.
   ```bash
   # docker pull centos:latest
   ```
1. Create a temporary container.
   ```bash
   # docker run -it -d -p 29003:29003 -v /root/work/:/mydata --name centos-sss centos:latest /bash
   ```
1. Attach the temporary container.
   ```bash
   # docker exec -it centos-sss bash
   ```
1. If your environment behind a proxy server, edit yum.conf.
   ```bash
   # vi /etc/yum.conf
     :
   proxy=http://<your proxy server>:<port number>
   sslverify=false
   ```
1. Install iproute and sysvinit-tools.
   ```bash
   # yum -y install iproute
   # yum -y install sysvinit-tools
   ```
   - iproute is needed to run **ip** command.
   - sysinit-tools is need to run **pidof** command.
1. Install EXPRESSCLUSTER X SingleServerSafe.
   ```bash
   # cd /mydata
   # rpm -i clusterprosss-4.1.1-1.x86_64.rpm
   ```
1. Register licenses of EXPRESSCLUSTER X SingleServerSafe and Agent.
1. Copy /mydata/start.sh to /root/start.sh.
1. Exit from the container and stop it.
   ```bash
   # exit
   # docker stop centos-sss
   ```
1. Create SingleServerSafe container image.
   ```bash
   # docker commit centos-sss centos-sss
   # docker images centos-sss
    :
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   centos-sss          latest              bd0164aa83b3        2 weeks ago         357 MB
   ```
1. Create a container.
   ```bash
   # docker run -it -d -p 29103:29003 --name centos-sss01 centos-sss:latest /root/start.sh
   ```
1. On container host or the other machine, start web browser and access to the following URL.
   ```
   http://<IP address of the container host>:29103
   ```
1. Add some resouces and monitor resources as you like.
1. Add **clpcl -s** command to /root/start.sh.
   ```bash
   cd /opt/nec/clusterpro/etc/systemd
   ./clusterpro_evt.sh start
   ./clusterpro_trn.sh start
   ./clusterpro_alertsync.sh start
   ./clusterpro_webmgr.sh start
   clpcl -s --local
   tail -f /dev/null
   ```
1. Start cluster service form Cluster WebUI.

## Install SingleServerSafe on MariaDB Container
1. Create the script file as below and save it on a directory (e.g. /root/work) of container host.
   ```bash
   # start-mariadb.sh
   cd /opt/nec/clusterpro/etc/systemd
   ./clusterpro_evt.sh start
   ./clusterpro_trn.sh start
   ./clusterpro_alertsync.sh start
   ./clusterpro_webmgr.sh start
   clpcl -s --local
   mysqld
   ```
   And change file mode bits as below.
   ```bash
   # chmod 755 start-mariadb.sh
   ```
1. Create apt.conf file and save work directory (e.g. /root/work).
   ```
   Acquire::https::Verify-Peer "false";
   Acquire::https::Verify-Host "false";
   Acquire::http::proxy "http://<your proxy server>:<port umber>";
   Acquire::https::proxy "http://<your proxy server>:<port number>";
   ```
1. Create the directory to save database files (e.g. /root/mariadb-sss1).
1. Download MariaDB container.
   ```bash
   # docker pull mariadb
   ```
1. Create MariaDB container.
   ```bash
   docker run --name mariadb-sss -v /root/work/mariadb:/var/lib/mysql -v /root/work:/mydata -e MYSQL_ROOT_PASSWORD=<password> -p 3316:3306 -p 29013:29003 -d mariadb:latest
   ```
1. Attach the container.
   ```bash
   # docker exec -it mariadb-sss bash
   ```
1. Login MariaDB.
   ```bash
   # mysql -u root
   ```
1. Allow remote connection.
   ```bash
   > GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY 'your password' WITH GRANT OPTION;
   > GRANT ALL PRIVILEGES ON *.* TO root@"127.0.0.1" IDENTIFIED BY 'your password' WITH GRANT OPTION;
   > select user,host from mysql.user;
   +------+--------------+
   | User | Host         |
   +------+--------------+
   | root | %            |
   | root | 127.0.0.1    |
   | root | ::1          |
     :
   ```
1. Create a database for monitoring.
   ```bash
   > create database testdb;
   ```
1. Copy apt.conf from /mydata to /etc/apt/.
1. Update the package list and install libxml2.
   ```bash
   # apt-get update
   # apt-get install libxml2
   ```
1. Install EXPRESSCLUSTER X SingleServerSafe 4.1.
   ```bash
   # dpkg -i expressclssss-4.1.1-1.amd64.deb
   ```
1. Register licenses of EXPRESSCLUSTER X SingleServerSafe and Agent.
1. Run the following commands to enable Cluster WebUI.
   ```bash
   cd /opt/nec/clusterpro/etc/systemd
   ./clusterpro_evt.sh start
   ./clusterpro_trn.sh start
   ./clusterpro_alertsync.sh start
   ./clusterpro_webmgr.sh start
   ```
1. On container host or the other machine, start web browser and access to the following URL.
   ```
   http://<IP address of the container host>:29013
   ```
1. Add the following group, resource and monitor resource.
   - Group
     - Failover Group (failover)
       - Set the default parameter.
   - Resource
     - Exec resource (exec)
       - Set the default parameter.
   - Monitor Resource
     - MySQL Monitor Resource (mysqlw)
     
       |Parameter      |Value|
       |---------------|-----|
       |Target Resource|exec |
       |Database Name  |testdb|
       |IP Address     |127.0.0.1|
       |Port           |3306|
       |User Name      |your user account|
       |Password       |your user password|
       |Table          |mysqlwatch|
       |Library Path   |/usr/lib/x86_64-linux-gnu/libmysqlclient.so.20.3.13|
       |Recovery Action|Execute only the final action|
       |Recovery Target|LocalServer|
       |Execute Script before Final Action|Enable|
       |Final Action|No operation|
       - On **Script Settings**, add the following command to stop MariaDB.
         ```bash
         /bin/kill -s SIGKILL `pgrep mysqld`
         ```
     - User Mode Monitor Resource (userw)
       - **Remove this monitor resource.** 
1. Copy /mydata/start-mariadb.sh to /root/start-mariadb.sh.
1. Exit from the container and stop it.
   ```bash
   # exit
   # docker stop mariadb-sss
   ```
1. Create SingleServerSafe container image.
   ```bash
   # docker commit mariadb-sss mariadb-sss
   # docker images mariadb-sss
    :
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   mariadb-sss         latest              532458b18acb        About an hour ago   460 MB
   ```
1. Create a container.
   ```bash
   docker run -i --name mariadb-sss1 -v /root/work/mariadb:/var/lib/mysql -p 3326:3306 -p 29023:29003 -d mariadb-sss:latest /root/start-mariadb.sh
   ```
