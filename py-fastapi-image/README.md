<h1 align = 'center'>Python Image</h1>
<p align = 'center'>This guide provides you with a compact and efficient Dockerfile, designed specifically to generate Python images.</p>
 
The focus of this setup is to create a container to execute a Python FastAPI application. As an additional feature, it offers the ability to operate a Postgres container for data persistence if required. The key elements used in this approach are as follows:

1. *.dockerignore*. As the name says, here we want the files/folders that will be ignored by the Docker

 ```
 .git
 .vscode
 .any_optional_folder
 ```
 
 2. *Dockefile*. Here are the recipe to build the container that will runs our Python FastAPI application. 

```dockerfile

# Use an official Python runtime as a parent image
FROM python:3.9-slim-buster AS base

# Set environment varibles
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Set work directory in the container
WORKDIR /app

# Copy project requirement files
COPY ./requirements.txt ./
COPY .env ./
COPY alembic.ini .
COPY alembic ./alembic

# Install any needed packages and Python dependencies
RUN apt-get update && apt-get -y install libpq-dev gcc \
    && pip install --no-cache-dir --upgrade -r requirements.txt

# Copy the rest of the working directory contents into the container
COPY ./src /app/src

# Make port 8000 available to the world outside this container
EXPOSE 8000

# Run app when the container launches
CMD ["uvicorn", "src.api.main:app", "--host", "0.0.0.0", "--port", "8000"]


```

This Dockerfile begins by specifying a parent image `FROM python:3.9-slim-buster AS base`), which in this case is a lightweight Python 3.9 image. It then sets some environment variables to control how Python runs. The `WORKDIR` sets the working directory for any instructions that follow in the Dockerfile.

The Dockerfile then copies over the project requirements files and installs the necessary packages and Python dependencies (`RUN pip install --no-cache-dir --upgrade -r requirements.txt`).

After copying the remaining project files into the container, it specifies a network port on which the container will run (`EXPOSE 8000`). Finally, the Dockerfile instructs the container to run our FastAPI application using uvicorn upon launch (`CMD ["uvicorn", "src.api.main:app", "--host", "0.0.0.0", "--port", "8000"]`).

We use Docker and Dockerfiles to package up our applications into a standardised unit for software development. This approach guarantees that the application will always run the same, regardless of its environment. By defining everything our application needs to run in a Dockerfile, we ensure consistency, reliability, and reproducibility throughout our software's lifecycle.
