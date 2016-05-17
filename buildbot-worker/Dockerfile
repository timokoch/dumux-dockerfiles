# A latent buildbot worker with debian
FROM debian:latest
MAINTAINER Timo Koch

# get the package list
RUN apt-get update && apt-get dist-upgrade --no-install-recommends --yes && \
    apt-get install --no-install-recommends --yes \
    ca-certificates \
    vim \
    python-dev \
    python-pip \
    libffi-dev \
    libssl-dev \
    git \
    pkg-config \
    build-essential \
    gfortran \
    cmake \
    libopenmpi-dev \
    openmpi-bin \
    libsuitesparse-dev \
    libsuperlu-dev \
    libug-dev \
    libeigen3-dev \
    && apt-get clean

# create a data folder
RUN mkdir -p /data

# install and set up a virtualenv
RUN pip install virtualenv
RUN ["virtualenv", "/data/buildbot"]
WORKDIR /data/buildbot

# install buildbot master using pip in a virtual env
RUN ./bin/pip install --upgrade pip
RUN ./bin/pip install --pre buildbot-worker

# set the enviroment variables
# BUILDBOT_USER and BUILDBOT_PASSWORD
# from outside

# link the worker folder from host using volumes
# -v <path-to-worker-dir-on-host>:/data/buildbot/worker
# for the docker run command
VOLUME /data/buildbot/worker

# start worker
WORKDIR /data/buildbot/
CMD ["./bin/buildbot-worker", "start", "--nodaemon", "worker"]
