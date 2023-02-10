FROM ubuntu:22.04 as builder

ARG DEBIAN_FRONTEND=noninteractive

RUN apt update
RUN apt upgrade -y

# Add Dependencies for PySpark
RUN apt-get update && apt-get install -y curl vim wget software-properties-common  \
    unzip less ssh net-tools ca-certificates jq \
    python3 python3-pip python3-numpy python3-matplotlib python3-scipy python3-pandas python3-simpy

RUN update-alternatives --install "/usr/bin/python" "python" "$(which python3)" 1

RUN cd /root && curl https://download.java.net/java/GA/jdk11/13/GPL/openjdk-11.0.1_linux-x64_bin.tar.gz -o "JDK_11.tar.gz"  \
    && tar -zxvf JDK_11.tar.gz     \
    && mkdir -p /usr/local/jvm     \
    && mv ~/jdk-11.0.1 /usr/local/jvm

ENV JAVA_HOME /usr/local/jvm/jdk-11.0.1

RUN echo "export JAVA_HOME=/usr/local/jvm/jdk-11.0.1" >> ~/.bashrc
RUN echo "export PATH=$JAVA_HOME/bin:$PATH" >> ~/.bashrc

ARG MAVEN_VERSION=3.6.3
ARG USER_HOME_DIR="/root"
ARG BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries


RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
 && curl -fsSL -o /tmp/apache-maven.tar.gz ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
 && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
 && rm -f /tmp/apache-maven.tar.gz \
 && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

RUN cd /root && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
  && unzip /root/awscliv2.zip \
  && cd /root && ./aws/install

RUN cd /root && mkdir ".aws" && touch /root/.aws/credentials
RUN cd /root && mkdir "shared_drive"

RUN cd /root && rm -rf *

FROM builder as dev-env

ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

ENTRYPOINT ["tail", "-f", "/dev/null"]