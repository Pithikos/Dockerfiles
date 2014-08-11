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



# ---------------------- JENKINS --------------------------

# Install jenkins
ADD http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key jenkins.key
RUN apt-key add jenkins.key
RUN sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
RUN apt-get update -qq
RUN apt-get install -qqy jenkins

# Install plugins
# THIS WON'T WORK: The plugin seems to be based on a so called "slave" with docker.
#ADD http://updates.jenkins-ci.org/latest/docker-build-step.hpi /root/.jenkins/plugins/docker-build-step.jpi



# -------------------- SUPERVISOR -------------------------
#       (MUST be at the end of all other programs)

# Install supervisor
RUN apt-get install -qqy supervisor

# Make config file
RUN echo_supervisord_conf > /etc/supervisord.conf

# Add programs to start
RUN echo '[program:docker_daemon]'                          >> /etc/supervisord.conf
RUN echo 'command=/usr/bin/docker.io -d'                    >> /etc/supervisord.conf
RUN echo '[program:jenkins]'                                >> /etc/supervisord.conf
RUN echo 'command=java -jar /usr/share/jenkins/jenkins.war' >> /etc/supervisord.conf

# Start supervisor (and the registed processes)
CMD supervisord --nodaemon -c /etc/supervisord.conf
