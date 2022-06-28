# syntax=docker/dockerfile:1

# Use the official lightweight Python image.
# https://hub.docker.com/_/python
FROM python:3.10-slim

RUN apt update && apt -y upgrade && apt-get -y install curl gnupg2 software-properties-common
RUN curl -fsSL https://apt.releases.hashicorp.com/gpg | apt-key add -
RUN apt-add-repository "deb [arch=$(dpkg --print-architecture)] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
RUN apt update && apt install terraform

COPY . /

WORKDIR /import

# Install production dependencies.
RUN pip install --no-cache-dir -r requirements.txt
RUN pip install --no-cache-dir -r dialogflow-cx/requirements.txt

# Run the web service on container startup. Here we use the gunicorn
# webserver, with one worker process and 8 threads.
# For environments with multiple CPU cores, increase the number of workers
# to be equal to the cores available.
# Timeout is set to 0 to disable the timeouts of the workers to allow Cloud Run to handle instance scaling.
ENV PYTHONPATH=$PYTHONPATH:/import/dialogflow-cx
CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 --timeout 0 main:app