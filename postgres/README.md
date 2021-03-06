<h1 align = 'center'>Postgres Recipe</h1>
<p align = 'center'> Docker compose to contruct a database</p>

The *yaml* file, use to construct can be found in the folder. However, we choose to put in this readme file the instructions needs to run.

```yaml
version: '3' # yaml version

services: # containers representation
  name-postgres-database: # service name
    image: postgres # docker images used to crerate the container
    environment: # env variables 
      POSTGRES_PASSWORD: 'postgres'
    container_name: name-postgres-database # container name

    ports: # access ports to the container
      - '5432:5432'
    volumes: # where data will be storage in the db
      - .docker/dbdata:/var/lib/postgresql/data


```

We have ou compose, now we need do executer. So in ther terminal you can the following
```shell
docker compose up -d
```


## Starting with Postgres Containers

There is another option, if you want just run with as single docker command use
```bash
docker run --name postgresql -e POSTGRES_USER=myusername -e POSTGRES_PASSWORD=mypassword -p 5432:5432 -v /data:/var/lib/postgresql/data -d postgres

```
The commands in details are:
- **PostgreSQL** is the of the Docker container.
- **-e POSTGRES_USER** is the parameter that sets a unique username to the Postgres database.
- **-e POSTGRES_USER** iis the parameter that allows you to set the password of the Postgres database.
- **-p 5432:5432 ** is the parameter that establishes a connection between the Host Port and Docker Container Port. In this case, both ports are given as 5432, which indicates requests sent to the Host Ports will automatically redirect to the Docker Container Port. In addition, 5432 is also the same port where PostgreSQL will be accepting requests from the client
- **-v** the parameter that synchronizes the Postgres data with the local folder. This ensures that Postgres data will be safely present within the Home Directory even if the Docker Container is terminated.
- **-d** is the parameter that runs the Docker Container in the detached mode, i.e., in the background. If you accidentally close or terminate the Command Prompt, the Docker Container will still run in the background.
- **postgres** is the name of the Docker image that was previously downloaded to run the Docker Container.

To enter a Postgres container, you need to execute using the container name and enable psql, the command-line interface for Postgres.
```bash
docker exec -it [container_name] psql -U [postgres_user]
```
With this command you access the terminal, where it is possible to run SQL command to manage your database

If you follow theses steps are possible for you run the a Postgree db with nay issue. And persist you application in a database.
