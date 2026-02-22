# GEN-AI with Chainlit DevContainer Image
# Python with Chainlit, LangChain, LangGraph, transformers, and AI tools + uv package manager

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

# Install Python GEN-AI stack with uv
RUN uv pip install chainlit langchain langgraph transformers sentence-transformers faiss-cpu

# Install Chainlit CLI
RUN uv pip install chainlit

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
RUN uv pip install --user chainlit langchain langgraph transformers sentence-transformers

ENV PATH="/home/vscode/.local/bin:/home/vscode/.cargo/bin:$PATH"

CMD ["zsh"]