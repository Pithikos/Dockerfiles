FROM ubuntu:14.04
MAINTAINER manossef@gmail.com

# Install docker
RUN apt-get update -qq
RUN apt-get install -qqy docker.io

# Docker symlink
RUN ln -s /usr/bin/docker.io /usr/local/bin/docker

# Solve AUFS nested mount issue
VOLUME /var/lib/docker

# Run docker daemon and keep it alive
CMD service docker.io start &> status && tail -f status
