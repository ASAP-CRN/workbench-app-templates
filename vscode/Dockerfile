FROM lscr.io/linuxserver/code-server@sha256:7e9523734c003b6336781942df7b48aa6936a9df6931c12a19a1f7ad7858eeba

USER root

# Create expected folders
RUN mkdir -p /config/extensions \
    /config/data/User \
    /workspace \
    /mycode \
    && chown -R abc:abc /config /workspace /mycode

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