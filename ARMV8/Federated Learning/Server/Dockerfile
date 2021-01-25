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
    liblapack-doc libblas3 libhdf5-dev liblapack3 liblapack-dev libblas-dev libatlas-base-dev   

# Library components
RUN apt-get install -y \
    libavformat-dev libavcodec-dev libavdevice-dev libstdc++6 \
    libavutil-dev libswscale-dev libswresample-dev libavfilter-dev \
    libgle3  git libopus-dev libvpx-dev libsrtp2-dev



RUN apt-get update -y && apt-get install apt-file -y && apt-get install -y python3-dev build-essential

RUN apt-get install -y pkg-config

# Library components
RUN apt-get install -y \
    libavformat-dev libavcodec-dev libavdevice-dev \
    libavutil-dev libswscale-dev libswresample-dev libavfilter-dev

RUN apt-get -y install ffmpeg python-all-dev libhdf5-dev  libzbar-dev libc-dev
RUN apt-get update && apt-get install -y \
        python-setuptools  libfreetype6-dev \
        liblcms2-dev libwebp-dev python-tk jq

RUN apt-get -y  install  make dpkg-dev tk-dev  libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev \
    libbz2-dev libexpat1-dev liblzma-dev  nano python3-pip python-openssl



RUN apt-get install -y freetype* libblas-dev liblapack-dev 



RUN apt-get -y install python3 swig graphviz-dev\
     libssl-dev  libffi6 libffi-dev python3-cffi python3-numpy libatlas-base-dev \
     libc-dev g++  libxml2 unixodbc unixodbc-dev portaudio19-dev python3-lxml\
     libxml2-dev libxslt1-dev zlib1g-dev gcc libpq-dev python3-wheel \
     autoconf libtool pkg-config python-opengl python-pil python-pyrex  idle-python2.7 \
     libicu-dev musl-dev\
     qt4-dev-tools qt4-designer libqtgui4 libqtcore4 libqt4-xml libqt4-test libqt4-script libqt4-network libqt4-dbus\
    python-qt4 python-qt4-gl libgle3  libopus-dev libvpx-dev libsrtp2-dev

RUN apt-get -y install  libldap2-dev libsasl2-dev  librtmp-dev\
    libpng-dev libjpeg-dev libvirt-dev libsqlite3-dev gfortran \ 
    cmake  wget curl graphicsmagick \
    libomp-dev libgraphicsmagick1-dev libboost-all-dev libgtk2.0-dev zip


#ENV BLAS=/usr/lib/aarch64-linux-gnu/libblas.so
#ENV LAPACK=/usr/lib/aarch64-linux-gnu/liblapack.so
ENV LD_LIBRARY_PATH=/usr/lib/aarch64-linux-gnu/liblapack.so
#ENV ATLAS=/usr/lib/aarch64-linux-gnu/libatlas.so
ENV LAPACK_LIBRARIES=/usr/lib/aarch64-linux-gnu/liblapack.so
ENV MKL_LAPACK_LIBRARIES=/usr/lib/aarch64-linux-gnu/liblapack.so

#RUN git clone https://github.com/xianyi/OpenBLAS.git
#RUN cd OpenBLAS/ && make clean && make CC=gcc FC=gfortran USE_OPENMP=1 NUM_THREADS=8 NO_AFFINITY=1 && make install
#ENV CMAKE_LIBRARY_PATH=/opt/OpenBLAS/include:/opt/OpenBLAS/lib:$CMAKE_LIBRARY_PATH
RUN wget https://github.com/xianyi/OpenBLAS/archive/v0.3.6.tar.gz \
	&& tar -xf v0.3.6.tar.gz \
	&& cd OpenBLAS-0.3.6/ \
	&& make BINARY=64 FC=$(which gfortran) USE_THREAD=1 \
	&& make PREFIX=/usr/lib/openblas install


#RUN wget https://github.com/Reference-LAPACK/lapack/archive/v3.9.0.tar.gz
#RUN tar -xvf  v3.9.0.tar.gz
#RUN  cd lapack-3.9.0/ && cp make.inc.example make.inc && ulimit -s unlimited && make && cp *.a path/to/lib
WORKDIR /opt/app
RUN echo $(printenv)
RUN echo $(whereis OpenBLAS)
RUN echo $(whereis liblapack)
RUN echo $(ls /opt/OpenBLAS)
RUN echo  $( strings /usr/lib/aarch64-linux-gnu/libstdc++.so.6 | grep GLIBCXX)
#RUN ln -s /usr/lib/aarch64-linux-gnu/liblapack.a /usr/lib/aarch64-linux-gnu/liblapack.so
RUN pip3 install Pillow
ENV ATLAS=/usr/lib/openblas/lib/libopenblas.so
#ENV BLAS=/usr/lib/openblas/lib/libopenblas.so
ENV LAPACK=/usr/lib/openblas/lib/libopenblas.so 
ENV NO_CUDA=1
ENV USE_CUDA=0
ENV CUDA_VISIBLE_DEVICES=""
ENV USE_LAPACK


RUN python3 -m pip install --upgrade pip setuptools wheel pyyaml conda numpy ninja cmake cffi
RUN apt-get install -y libopenblas-dev libblas-dev m4 cmake cython python3-dev python3-yaml python3-setuptools
ENV NO_CUDA=1
ENV NO_DISTRIBUTED=1
ENV NO_MKLDNN=1 
ENV NO_NNPACK=1
ENV NO_QNNPACK=1
#RUN pip3 install torch  -f https://download.pytorch.org/whl/torch_stable.html
#RUN pip3 install https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.7.0/torch-1.7.0-cp38-cp38-linux_aarch64.whl
RUN pip3 install  https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.7.1/torch-1.7.1-cp38-cp38-linux_aarch64.whl
#ENV CMAKE_LIBRARY_PATH=/opt/OpenBLAS/include:/opt/OpenBLAS/lib:$CMAKE_LIBRARY_PATH
#ENV CXXFLAGS="${CXXFLAGS} -D__STDC_FORMAT_MACROS"
#RUN git clone --branch v1.7.0 https://github.com/pytorch/pytorch.git pytorch-1.7.0 && cd pytorch-1.7.0/ && git submodule update --init --recursive && python3 setup.py build 
RUN python3 -E setup.py install


RUN python3 -m pip install  https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.7.1/torchvision-0.8.2-cp38-cp38-linux_aarch64.whl
RUN python3 -m pip install av
#RUN git clone https://github.com/OpenMined/PySyft.git && cd PySyft && pip3 install -e . && python3 setup.py install
#ENV LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/lib/openblas/lib/
RUN pip install -r requirements.txt
COPY . /opt/app

#creates work dir   
WORKDIR /app
#copy python script to the container folder app
COPY server_lstm.py /app/server_lstm.py

RUN chmod +x /app/server_lstm.py

CMD python /app/server_lstm.py $ROUNDS_ENV $CLIENTS_ENV $EPOCHS_ENV

