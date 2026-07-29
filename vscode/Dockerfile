FROM lscr.io/linuxserver/code-server@sha256:7e9523734c003b6336781942df7b48aa6936a9df6931c12a19a1f7ad7858eeba

# Gemini: https://open-vsx.org/extension/Google/geminicodeassist
# Claude: https://open-vsx.org/extension/Anthropic/claude-code
RUN apt-get update \
    && apt-get install -y --no-install-recommends jq \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p /config/extensions \
    && chown abc:abc /config /config/extensions

USER abc
ENV HOME=/config

RUN curl -fsSL 'https://open-vsx.org/api/Google/geminicodeassist/2.79.0' \
        | jq -r '.files.download' \
        | xargs curl -fL --compressed -o /tmp/geminicodeassist.vsix \
    && curl -fsSL 'https://open-vsx.org/api/Anthropic/claude-code/linux-x64/2.1.128' \
        | jq -r '.files.download' \
        | xargs curl -fL --compressed -o /tmp/claudecode.vsix \
    && /app/code-server/bin/code-server --extensions-dir /config/extensions --install-extension /tmp/geminicodeassist.vsix \
    && /app/code-server/bin/code-server --extensions-dir /config/extensions --install-extension /tmp/claudecode.vsix \
    && rm /tmp/geminicodeassist.vsix /tmp/claudecode.vsix \
    && mkdir -p /config/data/User \
    && echo '{"http.systemCertificatesNode":true}' > /config/data/User/settings.json

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

USER root

WORKDIR /config
