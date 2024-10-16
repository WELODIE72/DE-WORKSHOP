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
docker network create pg-network
```

Run Postgres (change the path)

```bash
docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v c:/Users/alexe/git/data-engineering-zoomcamp/week_1_basics_n_setup/2_docker_sql/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  --network=pg-network \
  --name pg-database \
  postgres:13
```

Run pgAdmin

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

python ingest_data.py \
  --user=root \
  --password=root \
  --host=localhost \
  --port=5432 \
  --db=ny_taxi \
  --table_name=yellow_taxi_trips \
  --url=${URL}
```

Build the image

```bash
docker build -t taxi_ingest:v001 .
```

On Linux you may have a problem building it:

```
error checking context: 'can't stat '/home/name/data_engineering/ny_taxi_postgres_data''.
```

You can solve it with `.dockerignore`:

* Create a folder `data`
* Move `ny_taxi_postgres_data` to `data` (you might need to use `sudo` for that)
* Map `-v $(pwd)/data/ny_taxi_postgres_data:/var/lib/postgresql/data`
* Create a file `.dockerignore` and add `data` there
* Check [this video](https://www.youtube.com/watch?v=tOr4hTsHOzU&list=PL3MmuxUbc_hJed7dXYoJw8DoCuVHhGEQb) (the middle) for more details 



Run the script with Docker

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

Run it:

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


### SQL 

Coming soon!
