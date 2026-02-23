# Data Engineering DevContainer Image
# Python 3.13 with Airflow, dbt, Spark, Kafka, and modern data engineering tools

FROM python:3.13-slim

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    gnupg \
    ca-certificates \
    apt-transport-https \
    rlwrap \
    netcat-openbsd \
    zsh \
    sudo \
    default-jdk \
    procps \
    && rm -rf /var/lib/apt/lists/*

# Install uv (Python package manager) using the recommended Docker method
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/
ENV UV_TOOL_BIN_DIR=/usr/local/bin

# Install Google Cloud SDK
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
RUN curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg
RUN apt-get update && apt-get install -y google-cloud-cli && rm -rf /var/lib/apt/lists/*

# Install Python data engineering stack with uv
RUN uv pip install --system --no-build-isolation \
    apache-airflow \
    dbt-core \
    dbt-bigquery \
    dbt-snowflake \
    dbt-postgres \
    polars \
    pyspark \
    kafka-python \
    confluent-kafka \
    pandas \
    numpy \
    sqlalchemy \
    psycopg2-binary \
    pymysql \
    pyarrow \
    fastparquet \
    s3fs \
    gcsfs \
    adlfs \
    fsspec \
    redis \
    celery \
    requests \
    pydantic \
    python-dotenv \
    rich-cli

# Install act (GitHub Actions runner)
RUN curl -s https://raw.githubusercontent.com/nektos/act/master/install.sh | bash -s -- -b /usr/local/bin

# Install additional data engineering tools
RUN uv pip install --system \
    great-expectations \
    soda-core \
    dlt \
    meltano \
    singer-python \
    prefect \
    dagster \
    dagit

# Install data validation and testing tools
RUN uv pip install --system \
    pytest \
    pytest-cov \
    black \
    ruff \
    mypy \
    pre-commit

# Install Jupyter for prototyping
RUN uv pip install --system \
    jupyterlab \
    jupyter-contrib-nbextensions \
    jupyter-nbextensions-configurator

# Install Spark (optional - can be heavy, uncomment if needed)
# ENV SPARK_VERSION=3.5.0
# ENV HADOOP_VERSION=3
# RUN wget https://archive.apache.org/dist/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz && \
#     tar -xzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz && \
#     mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} /opt/spark && \
#     rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz
# ENV SPARK_HOME=/opt/spark
# ENV PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin

# Install Docker CLI (for docker-in-docker functionality)
RUN curl -fsSL https://get.docker.com | sh

# Set up working directory
WORKDIR /workspace

# Configure shell
RUN chsh -s /bin/zsh

# Install oh-my-zsh
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# Set default user
RUN useradd -m vscode && \
    echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && \
    chown -R vscode:vscode /workspace

USER vscode

# Configure uv for the user
ENV PATH="/home/vscode/.cargo/bin:/usr/local/bin:$PATH"
RUN mkdir -p /home/vscode/.cargo/bin
RUN ln -s /root/.cargo/bin/uv /home/vscode/.cargo/bin/uv

# Install Python packages for the user with uv
RUN uv pip install --system \
    pandas \
    polars \
    sqlalchemy \
    pyarrow

ENV PATH="/home/vscode/.local/bin:/home/vscode/.cargo/bin:$PATH"

# Set up Airflow home
ENV AIRFLOW_HOME=/workspace/airflow

CMD ["zsh"]
