<h1 align = 'center'>Go and Postgres with docker</h1>
<p align = 'center'> Container to run a database and Go application</p>
 
 In this recipe, we will construct container to run a Go (Golang) application. And with bonus we can run a Postgres container, if you want to persist your data. Then, the following files are:

1. *.dockerignore*. As the name says, here we want the files/folders that will be ignored by the Docker

 ```
 .git
 .vscode
 .any_optional_folder
 ```
 
 2. *Dockefile*. Here are the recipe to build the container that will runs our Go application. 

```dockerfile
FROM golang:alpine AS build-env
ENV GOPATH /go
WORKDIR /go/src/github.com/github_user/app_name
COPY . /go/src/github.com/github_user/app_name
RUN cd /go/src/github.com/github_user/app_name && CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o build/mark github.com/github_user/app_name/cmd/main

FROM alpine

RUN apk update && apk add ca-certificates && rm -rf /var/cache/apk*
WORKDIR /app
COPY --from=build-env /go/src/github.com/github_user/app_name/build/app_name /app
COPY .env /app

EXPOSE 8080

ENTRYPOINT [ "./app_name" ]

```

3. Now, we will put our Dockerfile in a docker-compose to generate a database to persist our data. The choice here is to use Postgres, however you can choose any database. The following commands is 
```yml
version: '3'
services:
  database:
    image: "postgres"
    environment:
      - POSTGRES_USER=$DB_USER
      - POSTGRES_PASSWORD=$DB_PASSWORD
      - POSTGRES_DB=$DB_NAME
    ports:
      - "5432:5432"
    env_file:
      - ./.env
    volumes:
      - ./postgres-data:/var/lib/postgresql/data  


  server:
      build:
        context: .
        dockerfile: Dockerfile
      depends_on:
        - database
      networks:
        - default
      ports:
      - "8080:8080"
      env_file:
      - ./.env
volumes:
  data: 
```

We use here environment variables in, but you replace the values directly in the *docker-compose.yml*. 

As last you can run the following commands
```bash
docker-compose up -d --build
```
