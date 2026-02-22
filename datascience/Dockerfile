# Data Science DevContainer Image
# Python with Jupyter, pandas, numpy, matplotlib, scikit-learn + uv package manager

FROM python:3.11-slim

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    gnupg \
    ca-certificates \
    apt-transport-https \
    zsh \
    build-essential \
    libssl-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/*

# Install uv (Python package manager)
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.cargo/bin:$PATH"

# Install Python data science stack with uv
RUN uv pip install numpy pandas matplotlib scikit-learn jupyterlab seaborn plotly

# Install Jupyter extensions
RUN uv pip install jupyter-contrib-nbextensions jupyter-nbextensions-configurator

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
RUN uv pip install --user numpy pandas matplotlib scikit-learn jupyterlab

ENV PATH="/home/vscode/.local/bin:/home/vscode/.cargo/bin:$PATH"

CMD ["zsh"]