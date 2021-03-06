<h1 align = 'center'>Docker with a Python and Machine Learning</h1>
<p align = 'center'> Docker and Python</p>

In this recipe, we will construct a docker for Python. To start, we will make the following files.

1. *.dockerignore*

```
.git
.vscode
```
As the name said, here we have the files that we want that the docker ignore. Here is an example, if you're going to add more folders or files, you can add them in the following lines.

2. The next file is the Dockerfile, the content here is:
```dockerfile
# Specify your base image
FROM python:3.7.3-stretch
# create a work directory
RUN mkdir /app
# navigate to this work directory
WORKDIR /app
#Copy all files
COPY . .
# Install dependencies
RUN python -m pip install --upgrade pip
RUN pip install -r requirements.txt
# Run
CMD ["python","app.py"]
```
In particular a example it is need to write a requirements.txt. In this file we have all the python libraries to make the container work properly. The file for this example look-like this
```txt
Flask==1.1.4
Flask-AppBuilder==3.3.0
Flask-Babel==1.0.0
Flask-CacheBuster==1.0.0
Flask-Caching==1.10.1
Flask-Compress==1.10.1
Flask-Cors==3.0.10
Flask-JWT-Extended==3.25.1
Flask-Login==0.4.1
Flask-OpenID==1.2.5
Flask-Reuploaded==1.2.0
Flask-SQLAlchemy==2.5.1
Flask-WTF==0.14.3
MarkupSafe==1.1.1
Pillow==7.0.0
tensorflow==2.3.1
h5py==2.10.0
Keras==2.4.3
Keras-Preprocessing==1.1.2
Jinja2==2.11.3
```

If the container works properly you can access the port defined in you app.py( any name you like) .