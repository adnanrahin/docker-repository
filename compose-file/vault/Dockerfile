FROM ubuntu:20.04
RUN touch /etc/apt/apt.conf.d/99verify-peer.conf && \
  echo >>/etc/apt/apt.conf.d/99verify-peer.conf "Acquire { https::Verify-Peer false }" && \
  apt-get update && \
  apt-get install -y software-properties-common curl gnupg2 && \
  curl -fsSLk https://apt.releases.hashicorp.com/gpg | apt-key add - && \
  apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main" && \
  apt-get update && apt-get install -y \
  vault && \
  setcap cap_ipc_lock= /usr/bin/vault

COPY run.sh ./
CMD ./run.sh