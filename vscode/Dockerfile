FROM lscr.io/linuxserver/code-server@sha256:7e9523734c003b6336781942df7b48aa6936a9df6931c12a19a1f7ad7858eeba

USER root

# Install system dependencies, Python, and scientific package build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    jq \
    curl \
    ca-certificates \
    python3 \
    python3-venv \
    python3-dev \
    python3-pip \
    build-essential \
    git \
    cmake \
    gfortran \
    pkg-config \
    ninja-build \
    libopenblas-dev \
    liblapack-dev \
    libssl-dev \
    libffi-dev \
    libxml2-dev \
    libxslt1-dev \
    libjpeg-dev \
    zlib1g-dev \
    llvm \
    clang \
    libgl1 \
    libgl1-mesa-dri \
    libglib2.0-0 \
    libsm6 \
    libxrender1 \
    libxext6 \
    && rm -rf /var/lib/apt/lists/*

# Create expected folders
RUN mkdir -p /config/extensions \
    /config/data/User \
    /workspace \
    /mycode \
    && chown -R abc:abc /config /workspace /mycode

# Create Python virtual environment
RUN python3 -m venv /opt/venv \
    && /opt/venv/bin/python -m pip install --upgrade pip setuptools wheel meson

# Install Python requirements
COPY requirements.txt /tmp/requirements.txt

RUN /opt/venv/bin/pip install --no-cache-dir --prefer-binary -r /tmp/requirements.txt \
    && rm -rf /root/.cache/pip /tmp/requirements.txt \
    && chown -R abc:abc /opt/venv

ENV PATH="/opt/venv/bin:${PATH}"
ENV HOME=/config

# Install code-server extensions as the abc user
USER abc

RUN curl -fsSL 'https://open-vsx.org/api/Google/geminicodeassist/2.79.0' \
        | jq -r '.files.download' \
        | xargs curl -fL --compressed -o /tmp/geminicodeassist.vsix \
    && curl -fsSL 'https://open-vsx.org/api/Anthropic/claude-code/linux-x64/2.1.128' \
        | jq -r '.files.download' \
        | xargs curl -fL --compressed -o /tmp/claudecode.vsix \
    && /app/code-server/bin/code-server --extensions-dir /config/extensions --install-extension /tmp/geminicodeassist.vsix \
    && /app/code-server/bin/code-server --extensions-dir /config/extensions --install-extension /tmp/claudecode.vsix \
    && rm /tmp/geminicodeassist.vsix /tmp/claudecode.vsix \
    && echo '{"http.systemCertificatesNode":true}' > /config/data/User/settings.json

WORKDIR /config