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
    apt-get clean \
    apt-get lsof

RUN apt-get update && apt-get install -y curl vim wget \
    unzip less ssh net-tools ca-certificates jq

# Verify the installation
RUN python3.7 --version && pip3 --version

# Set the default Python version to 3.7
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1

# Set up a virtual environment (optional)
RUN python -m venv /venv

# Install Apache Airflow and dependencies
RUN pip3 install "apache-airflow[celery]==2.5.1" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.5.1/constraints-3.7.txt"
RUN pip3 install psycopg2-binary
RUN pip3 install apache-airflow-providers-amazon==7.4.1

# Set environment variables for Airflow
ENV AIRFLOW_HOME=/root/airflow
RUN mkdir -p ${AIRFLOW_HOME}/dags

# Initialize the Airflow database
WORKDIR /root/airflow

RUN airflow db init

# Expose port 8080 for the Airflow webserver
EXPOSE 18080

# Start the Airflow scheduler and webserver
CMD ["bash", "-c", "airflow scheduler & airflow webserver --port 18080"]
