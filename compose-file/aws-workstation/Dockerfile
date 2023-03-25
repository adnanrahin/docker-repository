FROM centos:7

RUN yum -y update --disablerepo=extras \
    && yum install --disablerepo=extras -y \
    ksh \
    python3 \
    java-1.8.0-openjdk \
    java-11-openjdk \
    crontabs \
    unzip \
    openssl \
    unzip \
    make \
    wget \
    ssh \
    less  \
    ssh  \
    net-tools ca-certificates \
    jq  \
    git-all

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

ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

ENTRYPOINT ["tail", "-f", "/dev/null"]