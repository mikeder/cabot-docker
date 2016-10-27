# Cabot Dockerfile
#
# https://github.com/shoonoise/cabot-docker
#
# VERSION 1.1

FROM ubuntu:trusty
MAINTAINER Alexander Kushnarev <avkushnarev@gmail.com>

# Prepare
RUN apt-get update
RUN apt-get install -y build-essential nodejs libpq-dev python-dev npm git curl libldap2-dev libsasl2-dev iputils-ping

RUN curl -OL https://bootstrap.pypa.io/get-pip.py
RUN python get-pip.py

# Deploy cabot
RUN git clone https://github.com/arachnys/cabot.git /cabot
ADD fixture.json /cabot/
ADD run.sh /cabot/
ADD gunicorn.conf /cabot/gunicorn.conf

# Install dependencies
RUN pip install -e /cabot/
# Make sure cabot_slack_alert is installed
RUN pip install cabot-alert-slack
RUN npm install --no-color -g coffee-script less@1.3 --registry http://registry.npmjs.org/

# Set env var
ENV PATH $PATH:/cabot/
ENV PYTHONPATH $PYTHONPATH:/cabot/

# Cabot settings
ENV DEBUG t
ENV DB_HOST db
ENV DB_PORT 5432
ENV DB_USER docker
ENV DB_PASS docker

RUN ["ln","-s","/usr/bin/nodejs","/usr/bin/node"]

EXPOSE 5000

WORKDIR /cabot/
CMD . /cabot/run.sh
