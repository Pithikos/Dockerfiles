FROM ubuntu:14.04
MAINTAINER manossef@gmail.com

# ---------------------- Django --------------------------
RUN apt-get update -qq
RUN apt-get install python-django -qqy

# Make user (REPLACE WITH A USERNAME FROM HOST)
# This is useful so that the files created with django are
# readable from the user in host.
RUN useradd manos
