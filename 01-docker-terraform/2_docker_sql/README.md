## Docker and SQL

### Install wget using Homebrew

```bash
✗ brew install wget
```

###  Download the data

```bash
wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz 
md csv_backup
mv yellow_tripdata_2021-01.csv.gz csv_backup
```


### Running Postgres with Docker

```bash
docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:13
```

The result should be 

```bash
13: Pulling from library/postgres
14c9d9d19932: Pull complete 
40c88e7faf79: Pull complete 
86552d6a2e14: Pull complete 
a986120aadb7: Pull complete 
b8f6f593e389: Pull complete 
76441873964f: Pull complete 
c00daffc086a: Pull complete 
3e872bee6899: Pull complete 
e5fad5150511: Pull complete 
9cdfe87ade9a: Pull complete 
80ba833808c5: Pull complete 
dc6814d84714: Pull complete 
028c14c36d19: Pull complete 
1de81e9381de: Pull complete 
Digest: sha256:5b074d58fdaebb94f317de869cc09404e0d7e1fba3f425702e46b26e719e7df5
Status: Downloaded newer image for postgres:13
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.utf8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/data ... ok
creating subdirectories ... ok
selecting dynamic shared memory implementation ... posix
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting default time zone ... Etc/UTC
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

initdb: warning: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    pg_ctl -D /var/lib/postgresql/data -l logfile start

waiting for server to start....2024-10-16 10:01:35.015 UTC [48] LOG:  starting PostgreSQL 13.16 (Debian 13.16-1.pgdg120+1) on aarch64-unknown-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
2024-10-16 10:01:35.017 UTC [48] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2024-10-16 10:01:35.023 UTC [49] LOG:  database system was shut down at 2024-10-16 10:01:34 UTC
2024-10-16 10:01:35.030 UTC [48] LOG:  database system is ready to accept connections
 done
server started
CREATE DATABASE


/usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*

2024-10-16 10:01:35.530 UTC [48] LOG:  received fast shutdown request
waiting for server to shut down....2024-10-16 10:01:35.531 UTC [48] LOG:  aborting any active transactions
2024-10-16 10:01:35.532 UTC [48] LOG:  background worker "logical replication launcher" (PID 55) exited with exit code 1
2024-10-16 10:01:35.532 UTC [50] LOG:  shutting down
2024-10-16 10:01:35.542 UTC [48] LOG:  database system is shut down
 done
server stopped

PostgreSQL init process complete; ready for start up.

2024-10-16 10:01:35.650 UTC [1] LOG:  starting PostgreSQL 13.16 (Debian 13.16-1.pgdg120+1) on aarch64-unknown-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
2024-10-16 10:01:35.651 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
2024-10-16 10:01:35.651 UTC [1] LOG:  listening on IPv6 address "::", port 5432
2024-10-16 10:01:35.654 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2024-10-16 10:01:35.674 UTC [63] LOG:  database system was shut down at 2024-10-16 10:01:35 UTC
2024-10-16 10:01:35.682 UTC [1] LOG:  database system is ready to accept connections
```


### Install CLI for Postgres

```bash
✗ pip install pgcli
```


### Connect CLI to Postgres
Using `pgcli` to connect to Postgres

```bash
pgcli -h localhost -p 5432 -u root -d ny_taxi
```

The result should be 

```bash
Password for root: 
Server: PostgreSQL 13.16 (Debian 13.16-1.pgdg120+1)
Version: 4.1.0
Home: http://pgcli.com
root@localhost:ny_taxi>
```

### NY Trips Dataset

Dataset:
* https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page
* https://www1.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf


### pgAdmin

Running pgAdmin

```bash
docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  dpage/pgadmin4
```

Connect to pgAdmin though the Browser

```bash
http://localhost:8080
```
Log in pgAdmin using the credentials:
- Email: admin@admin.com
- Password: root

Connect to Postgres database, create a new server in pgAdmin:

Right-click Servers in the sidebar and choose Create > Server.
  1. In the General tab, name your connection.
  2. In the Connection tab, enter:
    - Host: `host.docker.internal` (for a container)
    - Port: `5432`.
    - Database name: `ny_taxi`
    - Username: `root`
    - Password: `root`.
  3. Click Save to finish.


### Running Postgres and pgAdmin together

Create a network

```bash
➜  docker network create pg-network
```

List Docker networks
```bash
✗ docker network ls
NETWORK ID     NAME         DRIVER    SCOPE
8ca5765106bd   bridge       bridge    local
b08aca3a6781   host         host      local
c8479b3c4219   none         null      local
5867305035cb   pg-network   bridge    local
```

Run Postgres within the network
  
```bash
docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  --network=pg-network \
  --name pg-database \
  postgres:13
```

Run pgAdmin within the network

```bash
docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  --network=pg-network \
  --name pgadmin-2 \
  dpage/pgadmin4
```


### Data ingestion

Running locally

```bash
URL="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz"
export URL
python ingest_data.py \
  --user=root \
  --password=root \
  --host=localhost \
  --port=5432 \
  --db=ny_taxi \
  --table_name=yellow_taxi_trips \
  --url=${URL}
```

You should get 

```bash
Resolving github.com (github.com)... 20.26.156.215
Connecting to github.com (github.com)|20.26.156.215|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/f6895842-79e6-4a43-9458-e5b0b454a340?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20241016%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241016T153610Z&X-Amz-Expires=300&X-Amz-Signature=42a8f7156460e37c98371098a4b192c1689b079b914ee53d8595c75ea7038aec&X-Amz-SignedHeaders=host&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2021-01.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-10-16 16:36:10--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/f6895842-79e6-4a43-9458-e5b0b454a340?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20241016%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241016T153610Z&X-Amz-Expires=300&X-Amz-Signature=42a8f7156460e37c98371098a4b192c1689b079b914ee53d8595c75ea7038aec&X-Amz-SignedHeaders=host&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2021-01.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.110.133, 185.199.109.133, 185.199.108.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.110.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 25031880 (24M) [application/octet-stream]
Saving to: ‘output.csv.gz’

output.csv.gz                       100%[=================================================================>]  23.87M  34.1MB/s    in 0.7s    

2024-10-16 16:36:11 (34.1 MB/s) - ‘output.csv.gz’ saved [25031880/25031880]

inserted another chunk, took 5.226 second
inserted another chunk, took 5.256 second
inserted another chunk, took 5.997 second
/Users/mbikinaserge/Workspace/training/data-engineering-workshop/01-docker-terraform/2_docker_sql/ingest_data.py:49: DtypeWarning: Columns (6) have mixed types. Specify dtype option on import or set low_memory=False.
  df = next(df_iter)
inserted another chunk, took 6.130 second
inserted another chunk, took 3.934 second
Finished ingesting data into the postgres database
```


Build the data ingestion script as an image

```bash
docker build -t taxi_ingest:v001 .
```


```bash
✗ docker build -t taxi_ingest:v001 .
[+] Building 34.2s (11/11) FINISHED                                                                                      docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                     0.0s
 => => transferring dockerfile: 218B                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/python:3.9.1                                                                          1.7s
 => [auth] library/python:pull token for registry-1.docker.io                                                                            0.0s
 => [internal] load .dockerignore                                                                                                        0.0s
 => => transferring context: 2B                                                                                                          0.0s
 => [1/5] FROM docker.io/library/python:3.9.1@sha256:ca8bd3c91af8b12c2d042ade99f7c8f578a9f80a0dbbd12ed261eeba96dd632f                   17.7s
 => => resolve docker.io/library/python:3.9.1@sha256:ca8bd3c91af8b12c2d042ade99f7c8f578a9f80a0dbbd12ed261eeba96dd632f                    0.0s
 => => sha256:3cb0ab4a815457fb78fb84a41c66981420aaee5d06726859b77d250b65c1e990 2.22kB / 2.22kB                                           0.0s
...
 => => extracting sha256:7c492206888c962a8cd02d08cdbdc962f2a10b7e2373d1aca6ac94a817ca309c                                                0.0s
 => => extracting sha256:32ebfb02ddcff9e4c8d2b993f909b47b7141ba9fb8aa2719d96e6279e865f713                                                0.1s
 => [internal] load build context                                                                                                        0.0s
 => => transferring context: 2.46kB                                                                                                      0.0s
 => [2/5] RUN apt-get install wget                                                                                                       1.2s
 => [3/5] RUN pip install pandas sqlalchemy psycopg2                                                                                    13.0s 
 => [4/5] WORKDIR /app                                                                                                                   0.0s 
 => [5/5] COPY ingest_data.py ingest_data.py                                                                                             0.0s 
 => exporting to image                                                                                                                   0.5s 
 => => exporting layers                                                                                                                  0.5s 
 => => writing image sha256:d4f4a75239c293ba1beb76b3cacb78c1aa555bbffab6b209057d7d59865e3ead                                             0.0s 
 => => naming to docker.io/library/taxi_ingest:v001    
```

Run data ingestion script image with Docker

```bash
URL="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz"

docker run -it \
  --network=pg-network \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pg-database \
    --port=5432 \
    --db=ny_taxi \
    --table_name=yellow_taxi_trips \
    --url=${URL}
```

### Docker-Compose 

To run the all infrastructure with Docker compose :

```bash
docker-compose up
```

Run in detached mode:

```bash
docker-compose up -d
```

Shutting it down:

```bash
docker-compose down
```

Note: to make pgAdmin configuration persistent, create a folder `data_pgadmin`. Change its permission via

```bash
sudo chown 5050:5050 data_pgadmin
```

and mount it to the `/var/lib/pgadmin` folder:

```yaml
services:
  pgadmin:
    image: dpage/pgadmin4
    volumes:
      - ./data_pgadmin:/var/lib/pgadmin
    ...
```

