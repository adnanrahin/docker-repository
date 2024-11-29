FROM centos:8

# Install required dependencies
RUN yum -y update --disablerepo=extras \
    && yum install --disablerepo=extras -y \
    ksh \
    python3 \
    crontabs \
    unzip \
    openssl \
    unzip \
    make \
    wget \
    ssh \
    less  \
    net-tools \
    ca-certificates \
    git-all

# Install EPEL and jq
RUN yum install epel-release -y && yum install jq -y 

WORKDIR /root

# Install JDK 11
RUN cd /root && curl https://builds.openlogic.com/downloadJDK/openlogic-openjdk/11.0.25+9/openlogic-openjdk-11.0.25+9-linux-x64.tar.gz -o "JDK_11.tar.gz"  \
    && tar -zxvf JDK_11.tar.gz     \
    && mkdir -p /usr/local/jvm     \
    && mv ~/jdk-11.0.1 /usr/local/jvm

ENV JAVA_HOME /usr/local/jvm/jdk-11.0.1

RUN echo "export JAVA_HOME=/usr/local/jvm/jdk-11.0.1" >> ~/.bashrc
RUN echo "export PATH=$JAVA_HOME/bin:$PATH" >> ~/.bashrc

# Install Maven
ARG MAVEN_VERSION=3.6.3
ARG USER_HOME_DIR="/root"
ARG BASE_URL=https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/${MAVEN_VERSION}

RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
 && curl -fsSL -o /tmp/apache-maven.tar.gz ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
 && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
 && rm -f /tmp/apache-maven.tar.gz \
 && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

# Install AWS CLI
RUN cd /root && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
  && unzip /root/awscliv2.zip \
  && cd /root && ./aws/install

# Install kubectl (Kubernetes CLI)
RUN curl -LO https://dl.k8s.io/release/v1.30.6/bin/linux/amd64/kubectl \
    && chmod +x kubectl \
    && mv kubectl /usr/local/bin/

# Install kubectl completion (optional)
RUN curl -LO https://dl.k8s.io/release/v1.30.6/bin/linux/amd64/kubectl-completion \
    && chmod +x kubectl-completion \
    && mv kubectl-completion /usr/local/bin/

# Configure AWS CLI (optional, can be skipped if you don't need it)
RUN cd /root && mkdir ".aws" && touch /root/.aws/credentials

# Create a shared directory (optional, can be removed)
RUN cd /root && mkdir "shared_drive"

# Clean up unnecessary files (optional, depending on requirements)
RUN cd /root && rm -rf *

# Set environment variables for Maven and Kubernetes CLI
ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"
ENV PATH=$PATH:/usr/local/bin/kubectl

# Default entrypoint (container will run indefinitely)
ENTRYPOINT ["tail", "-f", "/dev/null"]
