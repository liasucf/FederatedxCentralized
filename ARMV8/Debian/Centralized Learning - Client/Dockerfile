#!/bin/bash
#Set the base image
FROM arm64v8/debian:buster-slim

ARG data
ENV DATA_ENV=${data}
ARG communication_rounds
ENV ROUNDS_ENV=${communication_rounds}

COPY requirements.txt /opt/app/requirements.txt
WORKDIR /opt/app


RUN apt-get update -y && apt-get install apt-file -y && apt-get install -y python3-dev build-essential

RUN apt-get install -y python-dev pkg-config

# Library components
RUN apt-get install -y \
    libavformat-dev libavcodec-dev libavdevice-dev \
    libavutil-dev libswscale-dev libswresample-dev libavfilter-dev

RUN apt-get -y install ffmpeg python-all-dev libhdf5-dev  libzbar-dev libc-dev
RUN apt-get update && apt-get install -y \
        python-setuptools  libfreetype6-dev \
        liblcms2-dev libwebp-dev python-tk jq
        
RUN apt-get -y  install  make dpkg-dev tk-dev  libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev \
    libbz2-dev libexpat1-dev liblzma-dev  nano python3-pip python-dev python-openssl 

RUN apt-get -y install python3 python3-dev build-essential swig graphviz-dev\
     libssl-dev  libffi6 libffi-dev python3-cffi python3-numpy libatlas-base-dev \
     libc-dev g++  libxml2 unixodbc unixodbc-dev portaudio19-dev python3-lxml\
     libxml2-dev libxslt1-dev zlib1g-dev gcc libpq-dev python3-wheel \
     autoconf libtool pkg-config python-opengl python-pil python-pyrex  idle-python2.7 \
     libicu-dev musl-dev\
     qt4-dev-tools qt4-designer libqtgui4 libqtcore4 libqt4-xml libqt4-test libqt4-script libqt4-network libqt4-dbus\
    python-qt4 python-qt4-gl libgle3  libopus-dev libvpx-dev libsrtp2-dev
RUN apt-get -y install  libldap2-dev libsasl2-dev libavdevice-dev libavfilter-dev librtmp-dev\
    libpng-dev libjpeg-dev libvirt-dev libsqlite3-dev gfortran \
    libavformat-dev libavdevice-dev libavcodec-dev libavfilter-dev libavutil-dev libswscale-dev libswresample-dev\
    cmake  wget curl graphicsmagick \
    libgraphicsmagick1-dev libboost-all-dev libgtk2.0-dev zip 

RUN apt-get install -y python-dev pkg-config
RUN apt-get -y  install python3-pip
RUN pip3 install Pillow

RUN  python3 -m pip install https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.6.0/torch-1.6.0-cp37-cp37m-linux_aarch64.whl
RUN python3 -m pip install https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.6.0/torchvision-0.7.0-cp37-cp37m-linux_aarch64.whl
RUN python3 -m pip install av
RUN pip3 install -r requirements.txt
COPY . /opt/app

#creates work dir   
WORKDIR /
#copy python script to the container folder app
COPY client_centralized_lstm.py client_centralized_lstm.py
ADD Data Data/
RUN chmod +x client_centralized_lstm.py


CMD python3 client_centralized_lstm.py $DATA_ENV $ROUNDS_ENV

