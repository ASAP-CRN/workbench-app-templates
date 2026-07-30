FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

ENV DEBIAN_FRONTEND=noninteractive
ENV PATH=/opt/venv/bin:$PATH



# Prepare apt and enable universe
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      software-properties-common ca-certificates apt-transport-https gnupg && \
    add-apt-repository universe && \
    apt-get update

# Install system dependencies required to build and run bioinformatics packages
RUN apt-get install -y --no-install-recommends \
    python3 python3-venv python3-dev python3-pip \
    build-essential git curl wget pkg-config ninja-build gfortran \
    libblas-dev liblapack-dev libatlas-base-dev libgmp-dev \
    libffi-dev libssl-dev \
    libxml2-dev libxslt1-dev zlib1g-dev \
    libgl1 libgl1-mesa-dri libglib2.0-0 libsm6 libxrender1 libxext6 && \
    rm -rf /var/lib/apt/lists/*

# Install ttyd from apt if available, otherwise download prebuilt binary
RUN apt-get update && apt-get install -y --no-install-recommends ttyd || \
    ( \
      wget -qO /tmp/ttyd.tar.gz https://github.com/tsl0922/ttyd/releases/download/1.7.4/ttyd.x86_64.tar.gz && \
      tar -xzf /tmp/ttyd.tar.gz -C /tmp && \
      mv /tmp/ttyd /usr/local/bin/ttyd && \
      chmod +x /usr/local/bin/ttyd && \
      rm -rf /tmp/ttyd* \
    ) && \
    rm -rf /var/lib/apt/lists/*

# Set up a virtual environment for Python and install required packages
FROM python:3.11-slim

# Install system build deps needed by scientific packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential git cmake gfortran \
    libopenblas-dev liblapack-dev libssl-dev libffi-dev pkg-config \
    libxml2-dev libxslt1-dev libjpeg-dev zlib1g-dev llvm clang \
    && rm -rf /var/lib/apt/lists/*

# create venv and upgrade pip
RUN python3 -m venv /opt/venv \
 && /opt/venv/bin/python -m pip install --upgrade pip setuptools wheel

# Copy requirements and install
COPY requirements.txt /tmp/requirements.txt
RUN /opt/venv/bin/pip install --no-cache-dir --prefer-binary -r /tmp/requirements.txt

ENV PATH="/opt/venv/bin:${PATH}"


# Ensure vscode user exists and set working directory
RUN useradd -m -s /bin/bash vscode || true
WORKDIR /home/vscode

RUN mkdir -p /workspace
RUN mkdir -p /mycode
COPY startupscript /mycode/startupscript
RUN chmod +x /mycode/startupscript/*.sh

# Start ttyd as the container entrypoint
CMD ["ttyd", "-W", "-p", "7681", "bash"]
