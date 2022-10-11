<h1 align = 'center'>MySQL Recipe</h1>
<p align = 'center'> Docker command to construct a database</p>

In this recipe, we will construct a simple container to run with a user and a password. The steps here follow the same structure of the PostgreSQL with Docker. Since, we want a simple container that contains MySQL the chosen is to run the command `docker run`. So, we have:
 
```bash
  docker run --name mysql_name -e MYSQL_ROOT_PASSWORD=root -e MYSQL_USER=myuser -e MYSQL_PASSWORD=mypassword -p 3306:3306 -v /data:/var/lib/mysql/data -d mysql
```

The commands in details are:
- **MySQL** is the of the Docker container.
- **-e MYSQL_ROOT_PASSWORD** is the parameter that sets a unique password to the root user.
- **-e MYSQL_USER** is the parameter that sets a unique username to the MySQL database.
- **-e MYSQL_PASSWORD** is the parameter that allows you to set the password of the MySQL database.
- **-p 3306:3306** is the parameter that establishes a connection between the Host Port and Docker Container Port. In this case, both ports are given as 3306, which indicates requests sent to the Host Ports will automatically redirect to the Docker Container Port. In addition, 3306 is also the same port where MySQL will be accepting requests from the client
- **-v** the parameter that synchronizes the MySQL data with the local folder. This ensures that MySQL data will be safely present within the Home Directory even if the Docker Container is terminated.
- **-d** is the parameter that runs the Docker Container in the detached mode, i.e., in the background. If you accidentally close or terminate the Command Prompt, the Docker Container will still run in the background.
- **mysql** is the name of the Docker image that was previously downloaded to run the Docker Container.

To enter a MySQL container, you need to execute using the container name and enable mysql, the command-line interface for Postgres.
```bash
docker exec -it [container_name] mysql -u [mysql_user] -p
```
With this command you access the terminal, where it is possible to run SQL command to manage your database


## Environment Variables
When you start the mysql image, you can adjust the configuration of the MySQL instance by passing one or more environment variables on the docker run command line. Do note that none of the variables below will have any effect if you start the container with a data directory that already contains a database: any pre-existing database will always be left untouched on container startup.

See also https://dev.mysql.com/doc/refman/5.7/en/environment-variables.html for documentation of environment variables which MySQL itself respects (especially variables like MYSQL_HOST, which is known to cause issues when used with this image).

### MYSQL_ROOT_PASSWORD
This variable is mandatory and specifies the password that will be set for the MySQL root superuser account. In the above example, it was set to my-secret-pw.

### MYSQL_DATABASE
This variable is optional and allows you to specify the name of a database to be created on image startup. If a user/password was supplied (see below) then that user will be granted superuser access (corresponding to GRANT ALL) to this database.

### MYSQL_USER, MYSQL_PASSWORD
These variables are optional, used in conjunction to create a new user and to set that user's password. This user will be granted superuser permissions (see above) for the database specified by the MYSQL_DATABASE variable. Both variables are required for a user to be created.

Do note that there is no need to use this mechanism to create the root superuser, that user gets created by default with the password specified by the MYSQL_ROOT_PASSWORD variable.

### MYSQL_ALLOW_EMPTY_PASSWORD
This is an optional variable. Set to a non-empty value, like yes, to allow the container to be started with a blank password for the root user. NOTE: Setting this variable to yes is not recommended unless you really know what you are doing, since this will leave your MySQL instance completely unprotected, allowing anyone to gain complete superuser access.

### MYSQL_RANDOM_ROOT_PASSWORD
This is an optional variable. Set to a non-empty value, like yes, to generate a random initial password for the root user (using pwgen). The generated root password will be printed to stdout (GENERATED ROOT PASSWORD: .....).

### MYSQL_ONETIME_PASSWORD
Sets root (not the user specified in MYSQL_USER!) user as expired once init is complete, forcing a password change on first login. Any non-empty value will activate this setting. NOTE: This feature is supported on MySQL 5.6+ only. Using this option on MySQL 5.5 will throw an appropriate error during initialization.

### MYSQL_INITDB_SKIP_TZINFO
By default, the entrypoint script automatically loads the timezone data needed for the CONVERT_TZ() function. If it is not needed, any non-empty value disables timezone loading.
