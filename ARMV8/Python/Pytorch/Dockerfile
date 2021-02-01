#!/bin/bash
#Set the base image
FROM arm64v8/python:3-slim

ARG communication_rounds
ARG n_clients
ARG n_epochs


ENV ROUNDS_ENV=${communication_rounds}

ENV CLIENTS_ENV=${n_clients}

ENV EPOCHS_ENV=${n_epochs}

COPY requirements.txt requirements.txt

#install requirements for torch
RUN apt-get update && apt-get install -y --no-install-recommends \
         gcc g++ build-essential \
         cmake \
         git \
         curl \
         ca-certificates \
         libjpeg-dev \
         libpng-dev gfortran liblapack-dev libopenblas-dev && \
     rm -rf /var/lib/apt/lists/*

RUN apt-get -y dist-upgrade

# Library components
#RUN apt-get install -y \
#    libavformat-dev libavcodec-dev libavdevice-dev libstdc++6 \
#    libavutil-dev libswscale-dev libswresample-dev libavfilter-dev \
#    libgle3  libopus-dev libvpx-dev libsrtp2-dev

RUN pip install cython hypothesis pyyaml pillow six

RUN echo $(which g++-8)
RUN echo $(CXX)
#ENV CC=$/usr/bin/gcc-8
#ENV CXX=$/usr/bin/g++-8
ENV LD_LIBRARY_PATH=/usr/lib/aarch64-linux-gnu/openblas/libblas.so

ENV USE_LAPACK=1
ENV USE_CUDA=0
ENV USE_FBGEMM=0
ENV USE_DISTRIBUTED=0
ENV BUILD_TEST=0

WORKDIR /app
# clone repo and build torch
RUN git clone https://github.com/pytorch/pytorch.git && cd ./pytorch && git submodule update --init --recursive && \
    pip install -r requirements.txt && python -E setup.py build

# copy built modules to site packages (for some reason, the built modules were not copied to site packages)
RUN cp -r /app/pytorch/build/lib.linux-aarch64-3.9/* /usr/local/lib/python3.9/site-packages/

RUN echo $(python -c "import torch")

#Intalling torchvision by wheel
RUN python3 -m pip install  https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.7.1/torchvision-0.8.2-cp38-cp38-linux_aarch64.whl
RUN python3 -m pip install av
#Installing pip requirements
RUN pip install -r requirements.txt

#creates work dir
#copy python script to the container folder app
COPY server_lstm.py server_lstm.py


# clean up
RUN rm -rf /app

RUN chmod +x server_lstm.py

CMD python server_lstm.py $ROUNDS_ENV $CLIENTS_ENV $EPOCHS_ENV

