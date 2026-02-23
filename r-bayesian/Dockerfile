# R Bayesian DevContainer Image
# Python 3.11 + R 4.3 + Bayesian statistics packages + radian console

FROM mambaorg/micromamba:ubuntu
USER root

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    gfortran \
    make \
    gcc \
    g++ \
    ca-certificates \
    curl \
    wget \
    git \
    libcurl4-openssl-dev \
    libxml2-dev \
    libssl-dev \
    libgit2-dev \
    libpng-dev \
    libjpeg-dev \
    libblas-dev \
    liblapack-dev \
    libudunits2-dev \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Copy environment file
COPY environment.yml /tmp/environment.yml

# Create conda environment with micromamba
RUN micromamba create -y -n r-bayesian -f /tmp/environment.yml -c conda-forge -c bioconda

# Clean up
RUN micromamba clean --all --yes

# Create radian wrapper
RUN echo '#!/bin/bash\nmicromamba run -n r-bayesian radian "$@"' > /usr/local/bin/radian && \
    chmod +x /usr/local/bin/radian

# Create R wrapper for convenience
RUN echo '#!/bin/bash\nmicromamba run -n r-bayesian R "$@"' > /usr/local/bin/R && \
    chmod +x /usr/local/bin/R

# Create Rscript wrapper for convenience
RUN echo '#!/bin/bash\nmicromamba run -n r-bayesian Rscript "$@"' > /usr/local/bin/Rscript && \
    chmod +x /usr/local/bin/Rscript

# Set default user
RUN useradd -m vscode && \
    echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER vscode

# Set environment variables
ENV PATH="/opt/conda/envs/r-bayesian/bin:$PATH"
ENV MAMBA_ROOT_PREFIX="/opt/conda"

# Configure radian
RUN mkdir -p /home/vscode/.config/radian && \
    echo 'options(radian.auto_match = TRUE)' > /home/vscode/.config/radian/profile && \
    echo 'options(radian.highlight_matching_bracket = TRUE)' >> /home/vscode/.config/radian/profile

CMD ["/bin/bash"]
