FROM ruby:2.2.2
MAINTAINER manossef@gmail.com

#
# NOTICE: Use this image ONLY for development
#


# You can configure the container with the environment variables
# below
# --------------------------------------------------------
#
# POSTGRES_USER - the user to be created for the database
#
# RAKE_COMMANDS - any rake commands you want to automatically
#                 run once the container is up
#
# --------------------------------------------------------
#

ENV POSTGRES_USER test
ENV RAKE_COMMANDS 'rake db:create && rake db:migrate'


# Set locale - essential for postgres to set the right
# encoding template.
RUN apt-get update && \
    apt-get install locales -qyy && \
    echo "en_GB.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen && \
    update-locale LC_ALL=en_GB.UTF-8 LANG=en_GB.UTF-8
ENV LC_ALL en_GB.UTF-8
ENV LANG en_GB.UTF-8
ENV LANGUAGE en_GB.UTF-8


# Install gems
COPY vendor  /src/vendor
COPY Gemfile /src/
COPY Gemfile.lock /src/
WORKDIR /src
RUN bundler install --local


# Install Postgres
RUN apt-get update && apt-get install -qqy \
    postgresql-contrib \
    postgresql-client \
    postgresql
RUN mkdir -p /var/run/postgresql/9.4-main.pg_stat_tmp &&\
    chown postgres:postgres /var/run/postgresql/9.4-main.pg_stat_tmp

# Allow local connections to postgres server without passwords
RUN sed -i 's/md5/trust/'  /etc/postgresql/9.4/main/pg_hba.conf &&\
    sed -i 's/peer/trust/' /etc/postgresql/9.4/main/pg_hba.conf


# Supervisor
#
# We install and run supervisord in order to be able and run multiple
# processes in the same container (ie. rails + postgres server).
#
# Notice also that postgres_init uses a sleep in the front to have
# supervisor wait a bit before trying to run the commands. It is normal
# for init_tasks to retry run the command (until postgres is running) and
# eventually give up.
#
RUN apt-get install supervisor -qy
RUN echo '\
[supervisord]                                                            \n\
logfile=/src/log/supervisord                                             \n\
                                                                         \n\
[program:postgres]                                                       \n\
user=postgres                                                            \n\
command=/usr/lib/postgresql/9.4/bin/postgres -D /etc/postgresql/9.4/main \n\
                                                                         \n\
[program:rails]                                                          \n\
command=rails server --port 3000 --binding 0.0.0.0                       \n\
                                                                         \n\
[program:init_tasks]                                                     \n\
user=postgres                                                            \n\
command=bash -c "sleep 0.4 &&                                            \n\
                 createuser $POSTGRES_USER -s &&                         \n\
                 bash -c \"$RAKE_COMMANDS\""                             \n\
' > /etc/supervisord.conf


WORKDIR /src
CMD supervisord --nodaemon -c /etc/supervisord.conf
