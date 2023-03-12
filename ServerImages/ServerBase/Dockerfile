# Setup follows this guide [Using ubuntu docker images](https://octopus.com/blog/using-ubuntu-docker-image)
FROM ubuntu:22.04
ARG TZ="ETC/UTC+1"

# Don't install default recomended apt ubuntu packages [Reducing docker sizes](https://ubuntu.com/blog/we-reduced-our-docker-images-by-60-with-no-install-recommends)
RUN echo 'APT::Install-Suggests "0";' >> /etc/apt/apt.conf.d/00-docker
RUN echo 'APT::Install-Recommends "0";' >> /etc/apt/apt.conf.d/00-docker

RUN DEBIAN_FRONTEND=noninteractive \
  apt-get update \
  #ADD NEEDED DEFAULT PACKAGES HERE, probably needs [curl, wget, software-properties-common]
  #&& apt-get install -y PACKAGE-NAME \
  && rm -rf /var/lib/apt/lists/*

# Creating a gameserver user [ubuntu useradd](https://manpages.ubuntu.com/manpages/jammy/en/man8/useradd.8.html)
RUN useradd -ms /bin/bash gameserver
USER gameserver