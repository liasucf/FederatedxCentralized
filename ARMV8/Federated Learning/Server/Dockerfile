#!/bin/bash
#Set the base image
FROM arm64v8/python:3.8-slim-buster

ARG communication_rounds
ARG n_clients
ARG n_epochs


ENV ROUNDS_ENV=${communication_rounds}

ENV CLIENTS_ENV=${n_clients}

ENV EPOCHS_ENV=${n_epochs}

COPY requirements.txt /opt/app/requirements.txt


RUN echo "deb http://httpredir.debian.org/debian/ jessie main contrib non-free" >> /etc/apt/sources.list
RUN echo "deb-src http://httpredir.debian.org/debian/ jessie main contrib non-free" >> /etc/apt/sources.list
RUN echo "deb http://security.debian.org/debian-security jessie/updates main" >> /etc/apt/sources.list 
RUN echo "deb http://ftp.de.debian.org/debian sid main " >> /etc/apt/sources.list
RUN apt-get update
RUN apt-get -y dist-upgrade
RUN apt-get -y install libstdc++6


RUN apt-get install -y \
    ffmpeg  python3-dev build-essential pkg-config cmake python-setuptools  libfreetype6-dev  libpng-dev libjpeg-dev\
    freetype* libblas-dev liblapack-dev libicu-dev musl-dev libssl-dev gcc  g++ libxml2 unixodbc unixodbc-dev  gfortran 

# Library components
RUN apt-get install -y \
    libavformat-dev libavcodec-dev libavdevice-dev libstdc++6 \
    libavutil-dev libswscale-dev libswresample-dev libavfilter-dev \
    libgle3  libopus-dev libvpx-dev libsrtp2-dev

WORKDIR /opt/app

RUN echo  $( strings /usr/lib/aarch64-linux-gnu/libstdc++.so.6 | grep GLIBCXX)

RUN pip3 install Pillow
RUN  python3 -m pip install  https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.6.0/torch-1.6.0-cp38-cp38-linux_aarch64.whl
RUN python3 -m pip install  https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.6.0/torchvision-0.7.0-cp38-cp38-linux_aarch64.whl
RUN python3 -m pip install av
    
RUN pip install -r requirements.txt
COPY . /opt/app

#creates work dir   
WORKDIR /app
#copy python script to the container folder app
COPY server_lstm.py /app/server_lstm.py

RUN chmod +x /app/server_lstm.py


CMD python /app/server_lstm.py $ROUNDS_ENV $CLIENTS_ENV $EPOCHS_ENV

