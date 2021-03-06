# build from existing torch dockerfile
FROM kaixhin/torch
MAINTAINER Kyle F <kylef@lab41.org>

# reset working directory
WORKDIR $HOME
ENV DEBIAN_FRONTEND noninteractive

# update
RUN apt-get update

# install utilities
RUN apt-get install --assume-yes  vim less net-tools inetutils-ping curl git telnet nmap socat dnsutils netcat tree htop unzip sudo software-properties-common wget

# install python foundation
RUN apt-get install --assume-yes  build-essential python-dev python-pip liblapack-dev libatlas-dev gfortran libfreetype6 libfreetype6-dev libpng12-dev python-lxml libyaml-dev g++ libffi-dev

# install lua foundation
RUN apt-get install --assume-yes lua5.2 libzmq3-dev

# update ipython
RUN apt-get remove --purge --assume-yes ipython && \
    pip install ipython jinja2 tornado jsonschema terminado
ENV IPYTHONDIR /ipython
RUN mkdir /ipython && \
    ipython profile create nbserver

# download tutorials
RUN git clone https://github.com/torch/tutorials.git

# download iTorch
RUN git clone https://github.com/facebook/iTorch.git && \
    cd iTorch && \
    luarocks make

# install unsupervised learning package
RUN git clone https://github.com/koraykv/unsup.git && \
    cd unsup && \
    luarocks make

# setup graph package
RUN apt-get install --assume-yes graphviz && \
    git clone https://github.com/torch/graph.git && \
    cd graph && \
    luarocks make

# setup nngraph package
RUN git clone https://github.com/torch/nngraph.git && \
    cd nngraph && \
    luarocks make

# expose iTorch notebook
EXPOSE 8888
VOLUME /data
WORKDIR /data
CMD ["itorch","notebook","--ip='*'"]
