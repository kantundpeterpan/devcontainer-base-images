# Data Science Extended DevContainer Image
# Python 3.13 with Jupyter, pandas, numpy, matplotlib, scikit-learn + uv package manager
# Extended with Polars, SQLite, SQLAlchemy, and additional SOTA packages

FROM python:3.13-slim

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    gnupg \
    ca-certificates \
    apt-transport-https \
    sudo \
    sqlite3 \
    libsqlite3-dev \
    && rm -rf /var/lib/apt/lists/*

# Install uv (Python package manager) using the recommended Docker method
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/
ENV UV_TOOL_BIN_DIR=/usr/local/bin

# Install Google Cloud SDK
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
RUN curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg
RUN apt-get update && apt-get install -y google-cloud-cli && rm -rf /var/lib/apt/lists/*

# Install Python data science stack with uv
RUN uv pip install --system \
    numpy pandas matplotlib scikit-learn jupyterlab seaborn plotly \
    polars sqlalchemy psycopg2-binary pymysql \
    pyarrow fastparquet s3fs gcsfs rich-cli

# Install act (GitHub Actions runner)
RUN curl -s https://raw.githubusercontent.com/nektos/act/master/install.sh | bash -s -- -b /usr/local/bin

# Install Jupyter extensions
RUN uv pip install --system \
    jupyter-contrib-nbextensions jupyter-nbextensions-configurator

# Install additional SOTA packages
RUN uv pip install --system \
    statsmodels xgboost lightgbm catboost \
    optuna scikit-optimize \
    imbalanced-learn category-encoders \
    great-expectations pydantic pandera

# Set default user
RUN useradd -m vscode && \
    echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER vscode

# Configure uv for the user
ENV PATH="/home/vscode/.cargo/bin:/usr/local/bin:$PATH"
RUN mkdir -p /home/vscode/.cargo/bin
RUN ln -s /root/.cargo/bin/uv /home/vscode/.cargo/bin/uv

# Install Python packages for the user with uv
RUN uv pip install --system \
    numpy pandas matplotlib scikit-learn jupyterlab \
    polars sqlalchemy

ENV PATH="/home/vscode/.local/bin:/home/vscode/.cargo/bin:$PATH"

CMD ["/bin/bash"]
