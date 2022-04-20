# コンテナホスト間クラスタの構築手順

## 評価環境
- CentOS Linux release 7.9.2009 (Core)
  - 3.10.0-1160.62.1.el7.x86_64
- Docker version 1.13.1, build 7d71120/1.13.1
- CLUSTERPRO X 5.0 for Linux (5.0.0-1)
```
+------------------------+
| Node #1 (node1)        |   +-------------+
| - CentOS 7             |   |             |
| - Docker               +---+ Mirror Disk |
| - CLUSTERPRO X 5.0     |   |             |
+------------------------+   +------|------+
                                    | Mirroring
+------------------------+   +------V------+
| Node #2 (node2)        |   |             |
| - CentOS 7             +---+ Mirror Disk |
| - Docker               |   |             |
| - CLUSTERPRO X 5.0     |   +-------------+
+------------------------+
```

## 事前準備
- インストール & 設定ガイドを参考に、クラスタを構築してください。
  - クラスタには以下のリソースを追加してください。
    - ミラーディスクリソース
      - マウントポイントとして、例えば /mnt/md1 を設定してください。
    - フローティング IP リソース
- [こちら](HowToInstallDocker.md)を参考に、Docker をインストールしてください。

## MariaDB のクラスタ化
1. node1, node2 にて、MariaDB のイメージをダウンロードしてください。
   ```sh
   docker pull mariadb
   ```
1. node1, node2 にて MariaDB のコンテナイメージがあることを確認してください。
   ```sh
   docker images
   ```
   ```
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   docker.io/mariadb   latest              100166b773f8        2 weeks ago         414 MB
   ```
1. CLUSTERPRO のフェイルオーバグループを node1 で起動してください。
1. node1 で以下のコマンドを実行してください。
   ```sh
   docker run --name mariadb1 -v /mnt/md1:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=watch -p 3306:3306 -d mariadb:latest
   ```
1. MariaDB のコンテナが起動していることを確認してください。
   ```sh
   docker ps
   ```
   ```
   CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
   f458ca16698c        mariadb:latest      "docker-entrypoint..."   6 seconds ago       Up 5 seconds        0.0.0.0:3306->3306/tcp   mariadb1
   ```
1. コンテナ mariadb1 で bash を起動します。
   ```sh
   docker exec -it mariadb1 bash
   ```
1. MariaDB が起動していることを確認します。
   ```
   root@f458ca16698c:/# mysql -u root -p
   Enter password:
   Welcome to the MariaDB monitor.  Commands end with ; or \g.
   Your MariaDB connection id is 4
   Server version: 10.7.3-MariaDB-1:10.7.3+maria~focal mariadb.org binary distribution
   
   Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   MariaDB [(none)]> show databases;
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | performance_schema |
   | sys                |
   | watch              |
   +--------------------+
   5 rows in set (0.003 sec)   

   MariaDB [(none)]> exit
   Bye
   root@f458ca16698c:/# exit
   exit
   ```
1. node1 で MariaDB のコンテナを停止してください。
   ```sh
   docker stop mariadb1
   ```
1. node2 で以下のコマンドを実行してください。
   ```sh
   docker create --name mariadb1 -v /mnt/md1:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=watch -p 3306:3306 -d mariadb:latest
   ```
   - node1 で実行したコマンドと異なり、docker create を使います。
1. CLUSTERPRO の Cluster WebUI を起動し、exec リソースを追加してください。また、start.sh, stop.sh を以下のように設定してください。
   - start.sh
     ```sh
     #! /bin/sh
     #***************************************
     #*              start.sh               *
     #***************************************
     
     #ulimit -s unlimited

     docker start mariadb1

     echo "EXIT"
     exit 0
     ```
   - stop.sh 
     ```sh
     #! /bin/sh
     #***************************************
     #*               stop.sh               *
     #***************************************
     
     #ulimit -s unlimited
     
     docker stop mariadb1
     
     echo "EXIT"
     exit 0
     ```
1. Cluster WebUI にて、MySQL モニタリソースを追加し、以下のパラメータを設定してください。
   - Database Name: watch
   - IP Address: 127.0.0.1
   - Port: 3306
   - User Name: root
   - Password: password
   - Table: mysqlwatch
   - Storage Engine: InnoDB
   - Library Path: /usr/lib64/mysql/libmysqlclient.so.18
1. クラスタの構成情報を反映し、クラスタを起動してください。
1. クラスタ起動後、フローティング IP アドレス経由で MariaDB にアクセスできることを確認してください。
   ```sh
   mysql -u root -p -h <フローティング IP アドレス> -P 3306
   ```
   - フェイルオーバを移動後、上記のコマンドでアクセスできることを確認してください。