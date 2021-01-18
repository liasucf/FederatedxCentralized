#!/bin/bash
#Set the base image
FROM arm64v8/python:3.7-stretch

ARG data
ENV DATA_ENV=${data}
ARG communication_rounds
ENV ROUNDS_ENV=${communication_rounds}

COPY requirements.txt /opt/app/requirements.txt
WORKDIR /opt/app

RUN apt-get -y install python3 python3-pip python-dev python3-dev \
     libssl-dev libffi-dev python3-cffi python3-numpy libatlas-base-dev \
     libxml2-dev libxslt1-dev zlib1g-dev gcc libpq-dev python3-wheel \
     autoconf libtool pkg-config python-opengl python-pil python-pyrex  idle-python2.7 \
     qt4-dev-tools qt4-designer libqtgui4 libqtcore4 libqt4-xml libqt4-test libqt4-script libqt4-network libqt4-dbus\
    python-qt4 python-qt4-gl libgle3  libopus-dev libvpx-dev libsrtp2-dev
RUN apt-get -y install libpq-dev  libldap2-dev libsasl2-dev libavdevice-dev libavfilter-dev libcurl4-gnutls-dev librtmp-dev\
    libpng-dev libjpeg-dev gfortran
    
RUN pip install -r requirements.txt
COPY . /opt/app

#creates work dir   
WORKDIR /
#copy python script to the container folder app
COPY client_fed_lstm.py client_fed_lstm.py
ADD Data Data/
RUN chmod +x client_fed_lstm.py


CMD python client_fed_lstm.py $DATA_ENV $ROUNDS_ENV
