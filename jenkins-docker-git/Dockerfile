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



# ------------------------ Git ----------------------------

# Install docker
RUN apt-get install -qqy git



# ---------------------- JENKINS --------------------------

# Install jenkins
ADD http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key jenkins.key
RUN apt-key add jenkins.key
RUN sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
RUN apt-get update -qq
RUN apt-get install -qqy jenkins

# Install Docker plugin
ADD http://updates.jenkins-ci.org/latest/docker-build-step.hpi /root/.jenkins/plugins/docker-build-step.jpi

# Install Github plugin
ADD http://updates.jenkins-ci.org/latest/github-api.hpi /root/.jenkins/plugins/github-api.jpi
ADD http://updates.jenkins-ci.org/latest/credentials.hpi /root/.jenkins/plugins/credentials.jpi
ADD http://updates.jenkins-ci.org/latest/ssh-credentials.hpi  /root/.jenkins/plugins/ssh-credentials.jpi
ADD http://updates.jenkins-ci.org/latest/git-client.hpi  /root/.jenkins/plugins/git-client.jpi
ADD http://updates.jenkins-ci.org/latest/scm-api.hpi  /root/.jenkins/plugins/scm-api.jpi
ADD http://updates.jenkins-ci.org/latest/git.hpi  /root/.jenkins/plugins/git.jpi
ADD http://updates.jenkins-ci.org/latest/ssh-agent.hpi  /root/.jenkins/plugins/ssh-agent.jpi
ADD http://updates.jenkins-ci.org/latest/github.hpi /root/.jenkins/plugins/github.jpi



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

# Start supervisor (and the registered processes)
CMD supervisord --nodaemon -c /etc/supervisord.conf
