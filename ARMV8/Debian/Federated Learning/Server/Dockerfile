#!/bin/bash
#Set the base image
FROM arm64v8/debian:buster-slim

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

RUN apt-get update -y && apt-get install apt-file -y && apt-get install -y python3-dev build-essential

RUN apt-get install -y python-dev pkg-config


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

RUN apt-get install -y \
        bash  ca-certificates less htop \
		rsync \
          musl  
RUN apt-get install -y \
    mlocate perl automake autoconf libtool* wget libopenblas-base libatlas3-base ffmpeg  python3-dev build-essential pkg-config cmake python-setuptools  libfreetype6-dev  libpng-dev libjpeg-dev\
    freetype* libblas-dev liblapack-dev libicu-dev musl-dev libssl-dev gcc  g++ libxml2 unixodbc unixodbc-dev  gfortran \
    liblapack-doc libblas3 libhdf5-dev liblapack3 libblas-dev libatlas-base-dev   

# Library components
RUN apt-get install -y \
    libavformat-dev libavcodec-dev libavdevice-dev libstdc++6 \
    libavutil-dev libswscale-dev libswresample-dev libavfilter-dev \
    libgle3  git libopus-dev libvpx-dev libsrtp2-dev

WORKDIR /opt/app
RUN echo $(printenv)
RUN echo $(whereis OpenBLAS)
RUN echo $(whereis liblapack)
RUN echo $(ls /opt/OpenBLAS)
RUN echo  $( strings /usr/lib/aarch64-linux-gnu/libstdc++.so.6 | grep GLIBCXX)


RUN pip3 install Pillow



RUN python3 -m pip install --upgrade pip setuptools wheel pyyaml conda numpy ninja cmake cffi
RUN apt-get install -y libopenblas-dev libblas-dev m4 cmake cython python3-dev python3-yaml python3-setuptools

#Installing Pytorch by wheel
RUN pip3 install  https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.7.1/torch-1.7.1-cp38-cp38-linux_aarch64.whl
#RUN git clone --branch v1.7.0 https://github.com/pytorch/pytorch.git pytorch-1.7.0 && cd pytorch-1.7.0/ && git submodule update --init --recursive && python3 setup.py build 

#Intalling torchvision by wheel
RUN python3 -m pip install  https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.7.1/torchvision-0.8.2-cp38-cp38-linux_aarch64.whl
RUN python3 -m pip install av
#Installing pip requirements
RUN pip install -r requirements.txt
COPY . /opt/app

#creates work dir   
WORKDIR /app
#copy python script to the container folder app
COPY server_lstm.py /app/server_lstm.py

RUN chmod +x /app/server_lstm.py

CMD python /app/server_lstm.py $ROUNDS_ENV $CLIENTS_ENV $EPOCHS_ENV

