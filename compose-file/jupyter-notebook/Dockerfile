# Use an official Python runtime as a parent image
FROM python:3.9-slim

# Set environment variables
ENV SPARK_VERSION=3.3.1
ENV HADOOP_VERSION=2
ENV JAVA_HOME=/usr/local/jvm/openjdk-11
ENV SPARK_HOME=/usr/local/spark
ENV PATH=$PATH:$JAVA_HOME/bin:$SPARK_HOME/bin:$SPARK_HOME/sbin

# Install system dependencies
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    tar \
    vim \
    procps \
    && rm -rf /var/lib/apt/lists/*

# Download and install Java 11
RUN mkdir -p /root && cd /root \
    && echo "Downloading OpenJDK 11..." \
    && wget https://download.java.net/openjdk/jdk11/ri/openjdk-11+28_linux-x64_bin.tar.gz -O JDK_11.tar.gz \
    && echo "Extracting OpenJDK 11..." \
    && tar -zxvf JDK_11.tar.gz \
    && echo "Moving OpenJDK 11 to /usr/local/jvm..." \
    && mkdir -p /usr/local/jvm \
    && mv jdk-11 /usr/local/jvm/openjdk-11

# Download and install Spark
RUN cd /root \
    && echo "Downloading Spark..." \
    && wget https://archive.apache.org/dist/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz -O spark.tgz \
    && echo "Extracting Spark..." \
    && tar -zxvf spark.tgz \
    && echo "Moving Spark to /usr/local/spark..." \
    && mkdir -p /usr/local/spark \
    && mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}/* /usr/local/spark \
    && rm -rf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}

# Set up Python environment
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt

# Ensure the Jupyter config directory exists
RUN mkdir -p /root/.jupyter && chmod -R 777 /root/.jupyter

# Copy Jupyter configuration files to /root/.jupyter
COPY jupyter_server_config.json /root/.jupyter/jupyter_server_config.json
COPY jupyter_notebook_config.py /root/.jupyter/jupyter_notebook_config.py

# Set correct permissions for Jupyter config files
RUN chmod 600 /root/.jupyter/jupyter_server_config.json /root/.jupyter/jupyter_notebook_config.py

# Install Spylon Kernel
RUN python -m spylon_kernel install --user

# Expose JupyterLab port
EXPOSE 8888
EXPOSE 4040
EXPOSE 4041

# Set up working directory
WORKDIR /workspace

# Start JupyterLab
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
