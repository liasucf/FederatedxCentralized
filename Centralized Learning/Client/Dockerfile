#!/bin/bash
#Set the base image
FROM python:3.7-stretch

ARG data
ENV DATA_ENV=${data}
ARG communication_rounds
ENV ROUNDS_ENV=${communication_rounds}

COPY requirements.txt /opt/app/requirements.txt
WORKDIR /opt/app
RUN pip install -r requirements.txt
COPY . /opt/app

#creates work dir   
WORKDIR /
#copy python script to the container folder app
COPY client_centralized_lstm.py client_centralized_lstm.py
ADD Data Data/
RUN chmod +x client_centralized_lstm.py


CMD python client_centralized_lstm.py $DATA_ENV $ROUNDS_ENV
