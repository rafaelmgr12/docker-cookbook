<h1 align = 'center'>Docker with a Node Application</h1>
<p align = 'center'> Docker and Node.js</p>


In this recipe, we will construct a docker for Node.js. To start, we will make the following files.

1. *.dockerignore*

```
node_modules
.git
.vscode
```
As the name said, here we have the files that we want that the docker ignore. Here is an example, if you're going to add more folders or files, you can add them in the following lines.

2. The next file is the Dockerfile, the content here is:
```dockerfile
FROM node:alpine  #node version

WORKDIR /usr/app

COPY package.json ./

RUN npm install

COPY . . 

EXPOSE 3333

CMD ["npm","run","dev"] 
```
3. Step 2 is sufficient if your application does not need a database. If necessary, it is recommended to use docker-compose, i.e., a YAML file. Therefore, we had
```yml  
version: "3.7"

services:
  database:
    image: postgres
    container_name: database_name
    restart: always
    ports: 
      - 5432:5432
    environment:
      - POSTGRES_USER=docker
      - POSTGRES_PASSWORD=passoword
      - POSTGRES_DB=db_name
    volumes:
      - pgdata:/data/postgres


  app:
    build: .
    container_name: name-container
    restart: always
    ports: 
      - 3333:3333
      - 9229:9229 #you can enter this port too to be able to use the debug
    volumes: 
      - .:/usr/app
      - /usr/src/app/node_modules

    links: 
      - database
    depends_on:
      - database



volumes:
  pgdata:
    driver: local
```
Here, We use the Postgres, however you can choose anyone you want.
## How to run :rocket:
To run this container, you need to install docker and open the terminal window and go to the directory folder. At last, run the following commands
```bash
docker build -f Dockerfile -t name_container:api .
```
This command runs the Dockerfile, however you can run the whole application using the docker-compose only using
```bash
docker-compose up -d
```
And your application you will run the port 3333.