# A latent buildbot worker with debian
FROM debian:latest
MAINTAINER Timo Koch

# get the package list
RUN apt-get update && apt-get dist-upgrade --no-install-recommends --yes && \
    apt-get install --no-install-recommends --yes \
    ca-certificates \
    build-essential \
    python-dev \
    python-pip \
    && apt-get clean

# create a data folder
RUN mkdir -p /data

# install and set up a virtualenv
RUN pip install virtualenv
RUN ["virtualenv", "/data/buildbot"]
WORKDIR /data/buildbot

# install buildbot master using pip in a virtual env
RUN ./bin/pip install --upgrade pip
# additional non-mandatory buildbot dependencies
RUN ./bin/pip install pyparsing \
                      docker-py==1.7 \
                      txrequests
# install buildbot
RUN ./bin/pip install --pre buildbot==0.9.0b9 \
                            buildbot-www==0.9.0b9

# the buildbot configuration is expected to be
# mounted as volume from host in into /data/buildbot/master
# set -v <path/to/hostmaster>:/data/buildbot/master
VOLUME /data/buildbot/master

# Setup running docker container buildbot process
# Make host port 8020 match container port 8020
EXPOSE 8020
EXPOSE 9989

# run master
CMD ["./bin/buildbot", "start", "--nodaemon", "master"]
