# WEEK 1

&nbsp;&nbsp;&nbsp;&nbsp; In this weeek I learned about docker and introduction of terraform, there are also homeworks that I have done using docker and terrafom that you can access in []().

## Docker

### PostgreSQL on Docker
&nbsp;&nbsp;&nbsp;&nbsp; First I created simple dockerfile with python 3:9 as a base of the container and **"bash" as an entrypoint** for the docker image. I run it to test the docker image with 

```docker run -it (docker image name)```

&nbsp;&nbsp;&nbsp;&nbsp; I also pull docker image for postgresql version 13 with this command ```docker pull postgres:13```. I downlaod the dataset from github [DE ZOOMCAMP DOCKER SQL WEEK 1](https://github.com/DataTalksClub/data-engineering-zoomcamp/tree/main/week_1_basics_n_setup/2_docker_sql). Then I run docker to create postgresql from docker using this command 

```docker run -it -e POSTGRES_USER="root" -e POSTGRES_PASSWORD="root" -e POSTGRES_DB="ny_taxis" -v C:/Users/ASUS/Documents/projects/DE_ZoomCamp/week_1/docker/ny_taxi_postgres_data:/var/lib/postgresql/data -p 30:5432 postgres:13```

&nbsp;&nbsp;&nbsp;&nbsp; Then after that I installed pgcli on my terminal adn run this command 

```pgcli -h localhost -p 30 -u root -d ny_taxis``` 

&nbsp;&nbsp;&nbsp;&nbsp; and it will show you pgcli in database ny_taxis.

### PGAdmin4 on Docker

&nbsp;&nbsp;&nbsp;&nbsp; After that I created the .ipynb file that I read the dataset using pandas convert the dataset into **ddl (data definition language)** and connect into postgreSQL that already specified using docker to create table from the dataset into the postgresql database. After finished insert record, now we can see the table in the database and used 

```SELECT COUNT(1) FROM yellow_taxi_data```

&nbsp;&nbsp;&nbsp;&nbsp; To see how much records that we have (yellow_taxi_data is our table name), I am using **sqlalchemy** to write sql queries to help me analyze the dataset and do my homework. 

&nbsp;&nbsp;&nbsp;&nbsp; Next install docker image for pgadmin4 using 

```docker pull dpage/pgadmin4```

&nbsp;&nbsp;&nbsp;&nbsp; Then run the container using this comman: 

```docker run -it -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" -e PGADMIN_DEFAULT_PASSWORD="root" -p 8080:80 dpage/pgadmin4```

&nbsp;&nbsp;&nbsp;&nbsp; **port 8080:80 means that the port will be mapping for 8080 in our host machine send to port 80 for our container port**. After that you can login into the docker admin in your browser with just typing ```localhost:8080```, after that sign in the pgadmin using email and password that have been defined when running the pgadmin.
After that you will be able to login into the pgadmin and just create a new server with specified the server name, host name, port, password. in this case we used **host name as localhost**, **username is root** and **password is root** because we have defined them when running the postgresql on docker container. But because we run the pgadmin inside the container we cannot used localhost because it will looking postgres in the same container as pgadmin and of course it cannot be found because postgres that we defined before is not in the container therefore we need to set **network** in both container (pgadmin and postgre container) to link the container of pgadmin and postgres. Use:

```docker network create pg-network```

&nbsp;&nbsp;&nbsp;&nbsp; pg-network is actually a network name that we want to created. After created the network we need to re-run both containers with **--network=pg-network** as an additional in parameter when running docker. And we also define name for the container therefore we remember name of the container.

**docker postgres:**

```docker run -it -e POSTGRES_USER="root" -e POSTGRES_PASSWORD="root" -e POSTGRES_DB="ny_taxis" -v C:/Users/ASUS/Documents/projects/DE_ZoomCamp/week_1/docker/ny_taxi_postgres_data:/var/lib/postgresql/data --network=pg-network -p 30:5432 --name pg-database postgres:13```

**docker pgadmin**

```docker run -it -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" -e PGADMIN_DEFAULT_PASSWORD="root" --network=pg-network -p 8080:80 --name pg-admin dpage/pgadmin4```

&nbsp;&nbsp;&nbsp;&nbsp; Then try again to create server with host name = pg-database for the container name in pg database, and the port of pg admin is same as the port postgre in docker container which is 5432. and now pg admin and postgre database are connected.

### argparse python

&nbsp;&nbsp;&nbsp;&nbsp; To more secure your database you can use **argparse library** to parse the arguments from  the command line in cli to to create your database like in [learn_argparse.py](). This script allow us to run python with command line arguments therefore people won't be able to know the environment for the database. Then after write the code, we can run python using:

```
python learn_argparse.py --user=root --password=root --host=localhost --port=30 --db=ny_taxis --table_name=yellow_taxi_data --url='https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz'
```

&nbsp;&nbsp;&nbsp;&nbsp; Then it will start to run the python script with downloading the data and save it into csv output and store the data into postgresql. After that we can create dockerfile to do this. after we create the dockerfile we can build image using this command:

```docker build -t argparse_docker:1.0 .```

&nbsp;&nbsp;&nbsp;&nbsp; Then using docker run to build the container, but because we using localhost as our host, to connect it with the another docker image like postgres we need **--network** to link the docker images and change the host name into the postgre container name in this case is **pg-database**. Test postgre image by running the container with the following command:

```docker run -it --network=pg-network argparse_docker:1.0 --user=root --password=root --host=pg-database --port=30 --db=ny_taxis --table_name=yellow_taxi_data --url='https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz'```

&nbsp;&nbsp;&nbsp;&nbsp; After that because we have several images and it will be hard to run them one by one, therefore we can use docker compose to help us run multiple containers at once. you can see the docker compose at this file [docker-compose.yaml](). Then you can run the docker compose using the following command, this case we used pg database and pgadmin database.

```docker-compose up```

and to shutdown the docker compose use:

```docker-compose down```

## Terraform