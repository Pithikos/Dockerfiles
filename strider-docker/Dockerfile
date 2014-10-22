FROM ubuntu:14.04
MAINTAINER manossef@gmail.com

EXPOSE 3000


# ----------------------- USER ---------------------------
RUN useradd -m strider


# ---------------------- DOCKER --------------------------
RUN apt-get update -qq
RUN apt-get install -qqy docker.io
RUN ln -s /usr/bin/docker.io /usr/local/bin/docker
VOLUME /var/lib/docker


# --------------------- Strider ---------------------------
RUN apt-get install -qqy nodejs nodejs-legacy npm git
RUN npm install -g strider


# ---------------------- Mongo ----------------------------
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
RUN echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | tee /etc/apt/sources.list.d/mongodb.list
RUN apt-get update -qq && apt-get install -qqy mongodb-org
RUN mkdir -p /data/db && chown -R strider /data


# -------------------- SUPERVISOR -------------------------
RUN apt-get install -qqy supervisor
RUN echo_supervisord_conf > /etc/supervisord.conf
RUN echo '[program:docker]'               >> /etc/supervisord.conf
RUN echo 'command=/usr/bin/docker.io -d'  >> /etc/supervisord.conf
RUN echo '[program:mongo]'                >> /etc/supervisord.conf
RUN echo 'command=/usr/bin/mongod'        >> /etc/supervisord.conf
RUN echo '[program:strider]'              >> /etc/supervisord.conf
RUN echo 'command=/usr/local/bin/strider' >> /etc/supervisord.conf


USER strider
CMD supervisord --nodaemon -c /etc/supervisord.conf
