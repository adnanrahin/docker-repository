FROM centos:8

# Fix CentOS 8 EOL issues by using vault.centos.org
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

# Install required dependencies
RUN yum -y update && yum install -y \
    ksh \
    python3 \
    crontabs \
    unzip \
    openssl \
    make \
    wget \
    openssh-clients \
    less \
    net-tools \
    ca-certificates \
    git

RUN dnf install -y bash-completion

# Install EPEL and jq
RUN yum install -y epel-release && yum install -y jq

WORKDIR /root

# EKSCTL Install
RUN curl --silent --location "https://github.com/eksctl-io/eksctl/releases/download/v0.207.0/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp && \
    mv /tmp/eksctl /usr/local/bin


# Install JDK 11
RUN cd /root && \
    curl -L https://builds.openlogic.com/downloadJDK/openlogic-openjdk/11.0.25+9/openlogic-openjdk-11.0.25+9-linux-x64.tar.gz -o "JDK_11.tar.gz" && \
    tar -zxvf JDK_11.tar.gz && \
    mkdir -p /usr/local/jvm && \
    mv openlogic-openjdk-11.0.25+9-linux-x64 /usr/local/jvm/jdk-11.0.25+9 && \
    ln -s /usr/local/jvm/jdk-11.0.25+9 /usr/local/jvm/jdk-11.0.1 && \
    rm JDK_11.tar.gz

ENV JAVA_HOME=/usr/local/jvm/jdk-11.0.1
ENV PATH=$JAVA_HOME/bin:$PATH

RUN echo "export JAVA_HOME=/usr/local/jvm/jdk-11.0.1" >> ~/.bashrc && \
    echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> ~/.bashrc

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

# Enable kubectl autocomplete
RUN echo "source /usr/share/bash-completion/bash_completion" >> ~/.bashrc && \
    echo "source <(kubectl completion bash)" >> ~/.bashrc && \
    echo "alias k=kubectl" >> ~/.bashrc && \
    echo "complete -F __start_kubectl k" >> ~/.bashrc

# Configure AWS CLI (optional)
RUN mkdir -p /root/.aws && touch /root/.aws/credentials

# Set environment variables
ENV MAVEN_HOME=/usr/share/maven
ENV MAVEN_CONFIG="$USER_HOME_DIR/.m2"
ENV PATH=$PATH:/usr/local/bin

# Default entrypoint (container runs indefinitely)
ENTRYPOINT ["tail", "-f", "/dev/null"]