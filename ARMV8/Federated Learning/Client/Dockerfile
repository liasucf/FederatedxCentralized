#!/bin/bash
#Set the base image
FROM arm64v8/python:3.8-slim-buster

ARG data
ENV DATA_ENV=${data}
ARG communication_rounds
ENV ROUNDS_ENV=${communication_rounds}

COPY requirements.txt /opt/app/requirements.txt
WORKDIR /opt/app
RUN apt-get update

RUN apt-get install -y \
    ffmpeg  python3-dev build-essential pkg-config libc-dev python-setuptools  libfreetype6-dev  libpng-dev libjpeg-dev\
    freetype* libblas-dev liblapack-dev libicu-dev musl-dev libssl-dev  libffi6 libffi-dev gcc  g++ libxml2 unixodbc unixodbc-dev  gfortran 

# Library components
RUN apt-get install -y \
    libavformat-dev libavcodec-dev libavdevice-dev \
    libavutil-dev libswscale-dev libswresample-dev libavfilter-dev \
    libgle3  libopus-dev libvpx-dev libsrtp2-dev

RUN pip3 install Pillow
RUN  python3 -m pip install  https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.6.0/torch-1.6.0-cp38-cp38-linux_aarch64.whl
RUN python3 -m pip install  https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.6.0/torchvision-0.7.0-cp38-cp38-linux_aarch64.whl
RUN python3 -m pip install av
    
RUN pip install -r requirements.txt
COPY . /opt/app

#creates work dir   
WORKDIR /
#copy python script to the container folder app
COPY client_fed_lstm.py client_fed_lstm.py
ADD Data Data/
RUN chmod +x client_fed_lstm.py


CMD python client_fed_lstm.py $DATA_ENV $ROUNDS_ENV
