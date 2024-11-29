FROM ubuntu:22.04 as builder

ARG DEBIAN_FRONTEND=noninteractive

RUN apt update
RUN apt upgrade -y

# Add Dependencies for PySpark
RUN apt-get update && apt-get install -y curl vim wget software-properties-common  \
    unzip less ssh net-tools ca-certificates jq git-all\
    python3 python3-pip openssl wget

RUN update-alternatives --install "/usr/bin/python" "python" "$(which python3)" 1

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

RUN cd /root && rm -rf *

FROM builder as ubuntu-aws-img

ENTRYPOINT ["tail", "-f", "/dev/null"]