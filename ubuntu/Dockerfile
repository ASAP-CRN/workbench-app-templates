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

# Create a dedicated virtual environment and install Python packages into it
RUN python3 -m venv /opt/venv && \
    /opt/venv/bin/python -m pip install --upgrade pip setuptools wheel meson && \
    /opt/venv/bin/pip install --no-cache-dir \
      jupyterlab \
      pandas \
      "numpy<2" \
      matplotlib \
      seaborn \
      pillow \
      scanpy \
      "scvi-tools>=0.20" \
      "umap-learn>=0.5.5" \
      "scikit-learn>=1.2" \
      "numba>=0.57" \
      "llvmlite>=0.40" \
      python-igraph \
      leidenalg \
      scib-metrics \
      ipywidgets \
      mygene \
      pynndescent \
      "cell_type_mapper @ git+https://github.com/alleninstitute/cell_type_mapper.git@rc/v1.5.2" \
      "abc_atlas_access[notebooks] @ git+https://github.com/alleninstitute/abc_atlas_access.git" && \
    rm -rf /root/.cache/pip

# Ensure vscode user exists and set working directory
RUN useradd -m -s /bin/bash vscode || true
USER vscode
WORKDIR /workspace

# Start ttyd as the container entrypoint
CMD ["ttyd", "-W", "-p", "7681", "bash"]
