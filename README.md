<p align="center">
  <a href="https://www.vertica.com/"><img src="https://user-images.githubusercontent.com/46227224/119309331-c8c8a200-bc8b-11eb-9fc6-5f674b3abe01.png" width=400/></a>
</p>

<p align="center"><a href="https://hub.docker.com/r/saadmairaj/vertica">Verica Community Edition Docker Image</a></p>

Fork of [jbfavre/docker-vertica](https://github.com/jbfavre/docker-vertica)

Docker images collection for Vertica database. Vertica is a column oriented database from Micro Focus. It's available with both a free community licence, and an enterprise one.

## Usage

You can use the image without persistent data store:

    docker run -p 5433:5433 saadmairaj/vertica

Or with persistent data store:

    docker run -p 5433:5433 -d \
               -v /path/to/vertica_data:/home/dbadmin/docker \
               saadmairaj/vertica

Or with custom database name (default is "docker") or database password (default is no password):

    docker run -p 5433:5433 -e DATABASE_NAME='notdocker' -e DATABASE_PASSWORD='foo123' saadmairaj/vertica

Default user is **dbadmin**

## Run SQL commands with VQSL

1. Run _saadmairtaj/vertica_ docker image

   ```bash
   $ docker run -p 5433:5433 saadmairaj/vertica
   ```

2. Connect to the vertica container using the _CONTAINER ID_ or _IMAGE_ name. Run the following command to the _CONTAINER ID_ or _IMAGE_ name

   ```bash
   $ docker ps
   CONTAINER ID   IMAGE                             COMMAND                  CREATED        STATUS        PORTS                                       NAMES
   3cd1d29c34f0   saadmairaj/vertica:10.1.1-RHEL6   "/opt/vertica/bin/do…"   24 hours ago   Up 24 hours   0.0.0.0:5433->5433/tcp, :::5433->5433/tcp   vigilant_clarke
   ```
 
   After getting the required _CONTAINER ID_ or _IMAGE_ name, run the following to get into the bash promt.

   ```bash
   $ docker exec -it 3cd1d29c34f0 bash
   [root@3cd1d29c34f0 /]# 
   ```

3. In the vertica bash, run `/opt/vertica/bin/vsql` to connect to the actual database
 
   ```bash
   [root@3cd1d29c34f0 /]# /opt/vertica/bin/vsql -U dbadmin
   Welcome to vsql, the Vertica Analytic Database interactive terminal.

   Type:  \h or \? for help with vsql commands
          \g or terminate with semicolon to execute query
          \q to quit

   dbadmin=>
   ```
   
   Run SQL commands from here, if you want to save the data then run the vertica image with [persistent data store](#usage)
   
   ```bash
   dbadmin=> \l
   List of databases
     name  | user_name 
   --------+-----------
    docker | dbadmin
   (1 row)
   ```

## How to build from Dockerfile

You have to get relevant Vertica package from my.vertica.com (registration mandatory).  
Save it in packages directory.

Then, use following command:

    docker build -f Dockerfile.<OS_codename>.<OS_version>_<Vertica_version> \
                 --build-arg VERTICA_PACKAGE=<vertica_package_name_matching_OS.deb/.rpm> \
                 -t saadmairaj/vertica:<Vertica_version>_<OS_codename>-<OS_version> .

Or have a look into `Makefile`.
