# WEEK 1

&nbsp;&nbsp;&nbsp;&nbsp; In this weeek I learned about docker and introduction of terraform, there are also homeworks that I have done using docker and terrafom that you can access in []().

## Docker WEEK 1

&nbsp;&nbsp;&nbsp;&nbsp; First I created simple dockerfile with python 3:9 as a base of the container and **"bash" as an entrypoint** for the docker image. I run it to test the docker image with ```docker run -it (docker image name)```. I also pull docker image for postgresql version 13 with this command ```docker pull postgres:13```. I downlaod the dataset from github [DE ZOOMCAMP DOCKER SQL WEEK 1](https://github.com/DataTalksClub/data-engineering-zoomcamp/tree/main/week_1_basics_n_setup/2_docker_sql). Then I run docker to create postgresql from docker using this command ```docker run -it -e POSTGRES_USER="root" -e POSTGRES_PASSWORD="root" -e POSTGRES_DB="ny_taxis" -v C:/Users/ASUS/Documents/projects/DE_ZoomCamp/week_1/docker/ny_taxi_postgres_data:/var/lib/postgresql/data -p 5432:5432 postgres:13```. Then after that I installed pgcli on my terminal adn run this command ```pgcli -h localhost -p5432 -u root -d ny_taxis``` and it will show you pgcli in database ny_taxis.

&nbsp;&nbsp;&nbsp;&nbsp; After that I created the .ipynb file that I read the dataset using pandas convert the dataset into **ddl (data definition language) and connect into postgreSQL that already specified using docker to create table from the dataset into the postgresql database. After finished insert record, now we can see the table in the database and used ```SELECT COUNT(1) FROM yellow_taxi_data``` to see how much records that we have (yellow_taxi_data is our table name)