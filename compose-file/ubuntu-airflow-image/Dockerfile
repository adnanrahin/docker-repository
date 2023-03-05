FROM ubuntu:22.04 as builder

ARG DEBIAN_FRONTEND=noninteractive

RUN apt update
RUN apt upgrade -y

# Add Dependencies for PySpark
RUN apt-get update && apt-get install -y curl vim wget software-properties-common  \
    unzip less ssh net-tools ca-certificates jq git-all\
    python3 python3-pip

#RUN pip3 install virtualenv
#RUN "virtualenv ~/airflow_env" && "source ~/airflow_env/bin/activate" && "mkdir ~/airflow" &&  \
#    pip3 install apache-airflow[gcp,sentry,statsd] && cd ~/airflow && airflow db init && "mkdir ~/airflow/dags"
#
#RUN cd ~/airflow && airflow users create --username admin --password admin --firstname Adnan --lastname Rahin --role Admin --email rahin.devtools@gmail.com
#
#RUN airflow scheduler -D && airflow webserver -D

FROM builder as dev-env

EXPOSE 8080

ENTRYPOINT ["tail", "-f", "/dev/null"]