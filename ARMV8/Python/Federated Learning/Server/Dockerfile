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

#Installing BLAS
RUN wget https://github.com/xianyi/OpenBLAS/archive/v0.3.6.tar.gz \
	&& tar -xf v0.3.6.tar.gz \
	&& cd OpenBLAS-0.3.6/ \
	&& make BINARY=64 FC=$(which gfortran) USE_THREAD=1 \
	&& make PREFIX=/usr/lib/openblas install



WORKDIR /opt/app
RUN echo $(printenv)
RUN echo $(whereis OpenBLAS)
RUN echo $(whereis liblapack)
RUN echo $(ls /opt/OpenBLAS)
RUN echo  $( strings /usr/lib/aarch64-linux-gnu/libstdc++.so.6 | grep GLIBCXX)


RUN pip3 install Pillow

ENV ATLAS=/usr/lib/openblas/lib/libopenblas.so
ENV BLAS=/usr/lib/openblas/lib/libopenblas.so
ENV LAPACK=/usr/lib/openblas/lib/libopenblas.so 
ENV NO_CUDA=1
ENV USE_CUDA=0
ENV CUDA_VISIBLE_DEVICES=""

#ENV LAPACK=/usr/lib/aarch64-linux-gnu/liblapack.so
ENV LD_LIBRARY_PATH=/usr/lib/aarch64-linux-gnu
#ENV ATLAS=/usr/lib/aarch64-linux-gnu/libatlas.so
ENV LAPACK_LIBRARIES=/usr/lib/aarch64-linux-gnu
ENV MKL_LAPACK_LIBRARIES=/usr/lib/aarch64-linux-gnu



RUN python3 -m pip install --upgrade pip setuptools wheel pyyaml conda numpy ninja cmake cffi
RUN apt-get install -y libopenblas-dev libblas-dev m4 cmake cython python3-dev python3-yaml python3-setuptools
ENV NO_CUDA=1
ENV NO_DISTRIBUTED=1
ENV NO_MKLDNN=1 
ENV NO_NNPACK=1
ENV NO_QNNPACK=1

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

