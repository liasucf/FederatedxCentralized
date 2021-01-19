#!/bin/bash
#Set the base image
FROM arm64v8/python:3.7-stretch

ARG communication_rounds
ARG n_clients
ARG n_epochs


ENV ROUNDS_ENV=${communication_rounds}

ENV CLIENTS_ENV=${n_clients}

ENV EPOCHS_ENV=${n_epochs}

COPY requirements.txt /opt/app/requirements.txt
WORKDIR /opt/app
RUN apt-get update
RUN apt-get upgrade
RUN apt-get -y install python3-dev build-essential apt-utils curl swig graphviz-dev\
     libssl-dev libffi-dev python3-cffi python3-numpy libatlas-base-dev \
     libc-dev g++  libxml2 unixodbc-dev portaudio19-dev python3-lxml\
     libxml2-dev libxslt1-dev zlib1g-dev gcc libpq-dev python3-wheel \
     autoconf libtool pkg-config python-opengl python-pil python-pyrex  idle-python2.7 \
     python-imaging python-pyside.qtopengl libicu-dev  musl-dev\
     qt4-dev-tools qt4-designer libqtgui4 libqtcore4 libqt4-xml libqt4-test libqt4-script libqt4-network libqt4-dbus\
    python-qt4 python-qt4-gl libgle3  libopus-dev libvpx-dev libsrtp2-dev
RUN apt-get -y install libpq-dev  libldap2-dev libsasl2-dev libavdevice-dev libavfilter-dev librtmp-dev\
    libpng-dev libjpeg-dev libvirt-dev libsqlite3-dev gfortran \
    git
    
RUN pip install -r requirements.txt
COPY . /opt/app

#creates work dir   
WORKDIR /app
#copy python script to the container folder app
COPY server_lstm.py /app/server_lstm.py

RUN chmod +x /app/server_lstm.py


CMD python /app/server_lstm.py $ROUNDS_ENV $CLIENTS_ENV $EPOCHS_ENV

