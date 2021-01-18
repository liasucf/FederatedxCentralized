#!/bin/bash
#Set the base image
FROM arm64v8/python:3.7-stretch

ARG communication_rounds
ARG n_clients
ARG n_epochs


ENV ROUNDS_ENV=${communication_rounds}

ENV CLIENTS_ENV=${n_clients}

ENV EPOCHS_ENV=${n_epochs}

COPY requirements.txt /opt/app/requirements.txt
WORKDIR /opt/app
RUN pip install -r requirements.txt
COPY . /opt/app

#creates work dir   
WORKDIR /app
#copy python script to the container folder app
COPY server_centralized_lstm.py /app/server_centralized_lstm.py

RUN chmod +x /app/server_centralized_lstm.py


CMD python /app/server_centralized_lstm.py $ROUNDS_ENV $CLIENTS_ENV $EPOCHS_ENV

