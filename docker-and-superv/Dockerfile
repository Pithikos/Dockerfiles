FROM ubuntu:14.04
MAINTAINER manossef@gmail.com



# ---------------------- DOCKER --------------------------

# Install docker
RUN apt-get update -qq
RUN apt-get install -qqy docker.io

# Docker symlink
RUN ln -s /usr/bin/docker.io /usr/local/bin/docker

# Solve AUFS nested mount issue
VOLUME /var/lib/docker



# -------------------- SUPERVISOR -------------------------
#       (MUST be at the end of all other programs)

# Install supervisor
RUN apt-get install -qqy supervisor

# Make config file
RUN echo_supervisord_conf > /etc/supervisord.conf

# Add programs to start
RUN echo '[program:docker_daemon]'       >> /etc/supervisord.conf
RUN echo 'command=/usr/bin/docker.io -d' >> /etc/supervisord.conf

# Start supervisor (and the registed processes)
CMD supervisord --nodaemon -c /etc/supervisord.conf
