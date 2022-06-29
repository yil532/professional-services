FROM python:3.10-slim

RUN apt-get update && apt-get install -y \
  build-essential
RUN apt update && apt -y upgrade && apt-get -y install curl gnupg2 software-properties-common
RUN curl -fsSL https://apt.releases.hashicorp.com/gpg | apt-key add -
RUN apt-add-repository "deb [arch=$(dpkg --print-architecture)] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
RUN apt update && apt install terraform

COPY . /
WORKDIR /import
RUN pip install --no-cache-dir -r requirements.txt

ENV PYTHONPATH=$PYTHONPATH:/import/dialogflow-cx
CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 --timeout 0 main:app