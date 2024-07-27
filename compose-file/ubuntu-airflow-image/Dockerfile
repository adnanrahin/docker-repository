# Use Ubuntu 22.04 as the base image
FROM ubuntu:22.04

# Set environment variables to prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Update the package list and install necessary dependencies
RUN apt-get update && \
    apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y python3.7 python3.7-dev python3.7-venv python3-pip && \
    apt-get clean

RUN apt-get update && apt-get install -y curl vim wget  \
    unzip less ssh net-tools ca-certificates jq

# Verify the installation
RUN python3.7 --version && pip3 --version

# Set the default Python version to 3.7
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1

# Set up a virtual environment (optional)
RUN python -m venv /venv

# Install dependencies
RUN pip3 install "apache-airflow[celery]==2.5.1" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.5.1/constraints-3.7.txt"
RUN pip3 install psycopg2-binary
RUN pip3 install apache-airflow-providers-amazon==7.4.1
RUN mkdir -p ~/airflow/dags

EXPOSE 8080

ENTRYPOINT ["tail", "-f", "/dev/null"]
