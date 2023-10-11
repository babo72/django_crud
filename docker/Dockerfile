FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install -y python3 python3-venv python3-pip
RUN python3 -m pip install --upgrade pip
RUN pip3 install --no-cache virtualenv

# create & activate virtual env
RUN python3 -m venv django-crud
RUN . /django-crud/bin/activate
RUN apt-get install -y python3-pip
RUN python3 -m pip install --upgrade pip

WORKDIR "/apps"

# include local files
COPY . .

# install pip packages
RUN pip install -r /apps/requirements.txt

# install required packages
RUN pip install --upgrade pip && pip3 install --no-cache django

WORKDIR "/apps/apps"

# migrate
RUN python3 manage.py migrate

EXPOSE 8000/tcp

# run server
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]


##########################################################################

# to build this image
# $ docker build --file docker/Dockerfile -t babo72/django-crud:latest .

# run image
# $ docker run -d --name django-crud-1 -p 8000:8000 babo72/django-crud

# shell connect
# $ docker exec -it django-crud-1 bash

###########################################################################
