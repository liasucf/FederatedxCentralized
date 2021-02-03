#!/bin/bash
#Set the base image
FROM arm64v8/python:3.6-slim

ARG communication_rounds
ARG n_clients
ARG n_epochs


ENV ROUNDS_ENV=${communication_rounds}

ENV CLIENTS_ENV=${n_clients}

ENV EPOCHS_ENV=${n_epochs}


#install requirements for torch
RUN apt-get update && apt-get install -y --no-install-recommends \
         gcc g++ build-essential \
         cmake \
         git \
         curl \
         ca-certificates \
         libjpeg-dev \
         libpng-dev gfortran liblapack-dev libopenblas-dev  pkg-config libffi-dev  libssl-dev 
     #rm -rf /var/lib/apt/lists/*

RUN apt-get -y dist-upgrade

# Library components
RUN apt-get install -y \
    libavformat-dev libavcodec-dev libavdevice-dev libstdc++6 \
    libavutil-dev libswscale-dev libswresample-dev libavfilter-dev \
    libgle3  libopus-dev libvpx-dev libsrtp2-dev

RUN pip install cython hypothesis pyyaml six
RUN pip install  Pillow==6.2.2
RUN pip install numpy==1.18.5
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
RUN git clone --branch v1.4.0a0 https://github.com/pytorch/pytorch.git  pytorch-1.4.0 && echo $(ls) && cd pytorch-1.4.0 && git submodule sync && git submodule update --init --recursive && \
    pip install -r requirements.txt && python -E setup.py bdist_wheel

#RUN echo $(ls /app/pytorch-1.4.0/dist)
RUN mv /app/pytorch-1.4.0/dist/torch-1.4.0a0+2c51e06-cp36-cp36m-linux_aarch64.whl  /app/pytorch-1.4.0/dist/torch-1.4.0-cp36-cp36m-linux_aarch64.whl
RUN pip3 install /app/pytorch-1.4.0/dist/torch-1.4.0-cp36-cp36m-linux_aarch64.whl
#RUN pip3 install https://mathinf.eu/pytorch/arm64/torch-1.3.0a0-cp37-cp37m-linux_aarch64.whl
#RUN pip3 install  https://mathinf.eu/pytorch/arm64/torchvision-0.5.0a0-cp37-cp37m-linux_aarch64.whl
# copy built modules to site packages (for some reason, the built modules were not copied to site packages)
#RUN echo $(ls /app/pytorch-1.4.0/build)
RUN cp -r /app/pytorch-1.4.0/build/lib.linux-aarch64-3.6/* /usr/local/lib/python3.6/site-packages/

RUN echo $(python -c "import torch")

#Intalling torchvision by wheel
#RUN python3 -m pip install  torchvision
RUN git clone --recursive https://github.com/pytorch/vision && cd vision && git checkout v0.5.0 && python3 setup.py install

#RUN apt-get install -y  pkg-config libffi-dev  libssl-dev

RUN python3 -m pip install av
#Installing pip requirements
COPY requirements.txt requirements.txt
#RUN pip install numpy==1.16.5
RUN pip install -r requirements.txt


#RUN git clone https://github.com/OpenMined/PySyft.git && cd PySyft && git checkout v0.2.6 && echo $(ls)  && python3 setup.py install
RUN pip3 install tblib syft_proto websocket websockets phe
RUN pip3 install --no-dependencies syft==0.2.6

#creates work dir
#copy python script to the container folder app
COPY server_lstm.py server_lstm.py


# clean up
#RUN rm -rf /app

RUN chmod +x server_lstm.py

CMD python server_lstm.py $ROUNDS_ENV $CLIENTS_ENV $EPOCHS_ENV

