FROM ubuntu:22.04 as builder

ARG DEBIAN_FRONTEND=noninteractive

RUN apt update
RUN apt upgrade -y

# Add Dependencies for PySpark

RUN mkdir ~/dependency

RUN apt-get update && apt-get install -y curl vim wget software-properties-common  \
    unzip less ssh net-tools ca-certificates jq git-all\
    python3 python3-pip

COPY ./dependency /root/dependency
WORKDIR /root/dependency

RUN pip install -r ~/dependency/requirements.txt

RUN mkdir -p ~/airflow/dags
RUN cd ~/airflow && airflow db init
RUN cd ~/airflow && airflow users create --username admin --password admin --firstname Adnan --lastname Rahin --role Admin --email rahin.devtools@gmail.com


FROM builder as airflow-container

EXPOSE 8080

ENTRYPOINT ["tail", "-f", "/dev/null"]