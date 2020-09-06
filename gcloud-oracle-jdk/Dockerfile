FROM ubuntu:latest

LABEL maintainer="vishal @t t a p p i n g t o a d c o m"

# Setting up env tz data
ARG DEBIAN_FRONTEND=noninteractive

# Upgrade the base distro
RUN apt-get update && apt-get upgrade -y

# Install some common tools and packages
RUN apt-get install -y software-properties-common \
        zip \
        unzip \
        git \
        ssh \
        curl \
        apt-transport-https \
        ca-certificates

# Docker
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" && \
    apt-get update && \
    apt-get install -y docker-ce

# Oracle JDK
RUN add-apt-repository ppa:linuxuprising/java -y && \
    apt-get update && \
    echo oracle-java14-installer shared/accepted-oracle-license-v1-2 select true | /usr/bin/debconf-set-selections && \
    apt-get install -y oracle-java14-installer && \
    apt-get install -y oracle-java14-set-default && \
    apt-get clean

# gcloud sdk
RUN . /etc/os-release && \
    export CLOUD_SDK_REPO="cloud-sdk-${UBUNTU_CODENAME}" && \
    echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
    apt-get update && \
    apt-get install -y \
        google-cloud-sdk \
        google-cloud-sdk-app-engine-go \
        google-cloud-sdk-app-engine-java \
        google-cloud-sdk-app-engine-python \
        google-cloud-sdk-bigtable-emulator \
        google-cloud-sdk-cbt \
        google-cloud-sdk-datalab \
        google-cloud-sdk-datastore-emulator \
        google-cloud-sdk-pubsub-emulator \
        kubectl && \
    apt-get clean

# make suitable for circle ci
RUN adduser circleci --home /home/circleci --gecos "" --disabled-password

USER circleci
