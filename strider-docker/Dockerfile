FROM ubuntu:14.04
MAINTAINER manossef@gmail.com

EXPOSE 3000
ENV IMAGE_NAME strider
ENV IMAGE_TYPE ci_software_suite


# ----------------------- USER ---------------------------
RUN useradd -m strider
RUN echo 'strider ALL=(ALL) NOPASSWD: /usr/bin/supervisord' >> /etc/sudoers
RUN groupadd -r docker
RUN gpasswd -a strider docker


# ---------------------- DOCKER --------------------------
RUN apt-get update -qq && apt-get install -qqy apt-transport-https
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
RUN sh -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"
RUN apt-get update -qq && apt-get install -qqy lxc-docker
RUN ln -s /usr/bin/docker.io /usr/local/bin/docker
VOLUME /var/lib/docker


# --------------------- Strider ---------------------------
RUN apt-get install -qqy nodejs nodejs-legacy npm git
RUN npm install -g strider
RUN mkdir /home/strider/.strider && chown -R strider /home/strider/.strider


# ---------------------- Mongo ----------------------------
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
RUN echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | \
    tee /etc/apt/sources.list.d/mongodb.list
RUN apt-get update -qq && apt-get install -qqy mongodb-org
RUN mkdir -p /data/db # && chown -R strider /data


# -------------------- SUPERVISOR -------------------------
RUN apt-get install -qqy supervisor
RUN echo_supervisord_conf > /etc/supervisord.conf
RUN echo '[program:docker]'                >> /etc/supervisord.conf
RUN echo 'command=service docker start'    >> /etc/supervisord.conf
RUN echo '[program:mongo]'                 >> /etc/supervisord.conf
RUN echo 'command=mongod'                  >> /etc/supervisord.conf
RUN echo '[program:strider]'               >> /etc/supervisord.conf
#RUN echo 'user=strider'                   >> /etc/supervisord.conf
RUN echo 'command=/usr/local/bin/strider'  >> /etc/supervisord.conf


USER strider
ENV HOME /home/strider
WORKDIR /home/strider/.strider

CMD sudo supervisord --nodaemon -c /etc/supervisord.conf
