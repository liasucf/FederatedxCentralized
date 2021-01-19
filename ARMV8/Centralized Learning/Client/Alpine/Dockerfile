FROM arm64v8/python:3.7.9-alpine

ARG data
ENV DATA_ENV=${data}
ARG communication_rounds
ENV ROUNDS_ENV=${communication_rounds}

COPY requirements.txt /opt/app/requirements.txt
WORKDIR /opt/app

RUN apt-get update
RUN apt-get install libpng-dev libjpeg-dev zlib1g-dev libssl-dev libffi-dev \
     libxml2-dev libxslt1-dev libopenblas-dev

RUN pip install Pillow==4.1.1

RUN  python3 -m pip install https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.6.0/torch-1.6.0-cp37-cp37m-linux_aarch64.whl
RUN python3 -m pip install https://github.com/KumaTea/pytorch-aarch64/releases/download/v1.6.0/torchvision-0.7.0-cp37-cp37m-linux_aarch64.whl

#RUN cd /usr/local/lib/python3.7/dist-packages/torch && mv _C.cpython-37m-arm-linux-gnueabi.so _C.so && \
    #mv _dl.cpython-37m-arm-linux-gnueabi.so _dl.so

RUN pip install --no-cache-dir -r requirements.txt

COPY . /opt/app

#creates work dir   
WORKDIR /
#copy python script to the container folder app
COPY client_centralized_lstm.py client_centralized_lstm.py
ADD Data Data/
RUN chmod +x client_centralized_lstm.py


CMD python client_centralized_lstm.py $DATA_ENV $ROUNDS_ENV





