# Fullstack DevContainer Image
# Python + Node + AI Tools + Cloud SDK + uv package manager

FROM python:3.11-slim

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
    build-essential \
    libssl-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/*

# Install uv (Python package manager)
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.cargo/bin:$PATH"

# Install Node.js (LTS)
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -
RUN apt-get install -y nodejs && rm -rf /var/lib/apt/lists/*

# Install Google Cloud SDK
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
RUN apt-get update && apt-get install -y google-cloud-cli && rm -rf /var/lib/apt/lists/*

# Install Python tools with uv
RUN uv pip install ruff black isort pytest

# Install act (GitHub Actions runner)
RUN curl -s https://raw.githubusercontent.com/nektos/act/master/install.sh | bash -s -- -b /usr/local/bin

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
RUN uv pip install --user ruff black isort pytest

ENV PATH="/home/vscode/.local/bin:/home/vscode/.cargo/bin:$PATH"

CMD ["zsh"]