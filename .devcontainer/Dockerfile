ARG NODE_VERSION=22
FROM mcr.microsoft.com/devcontainers/javascript-node:${NODE_VERSION}

ARG VARIANT=hugo_extended
ARG VERSION=latest

USER root

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ca-certificates curl git jq libstdc++6; \
    rm -rf /var/lib/apt/lists/*; \
    if [ "${VERSION}" = "latest" ]; then \
        curl -fsSL https://api.github.com/repos/gohugoio/hugo/releases/latest \
            -o /tmp/hugo-release.json; \
        VERSION="$(jq -er '.tag_name' /tmp/hugo-release.json)"; \
    fi; \
    VERSION="${VERSION#v}"; \
    ARCH="$(dpkg --print-architecture)"; \
    case "${ARCH}" in \
        amd64|arm64) ;; \
        *) echo "Unsupported architecture: ${ARCH}"; exit 1 ;; \
    esac; \
    curl -fSL --retry 3 \
        "https://github.com/gohugoio/hugo/releases/download/v${VERSION}/${VARIANT}_${VERSION}_linux-${ARCH}.tar.gz" \
        -o /tmp/hugo.tar.gz; \
    tar -xzf /tmp/hugo.tar.gz -C /usr/local/bin hugo; \
    chmod 0755 /usr/local/bin/hugo; \
    rm -f /tmp/hugo.tar.gz /tmp/hugo-release.json; \
    hugo version

EXPOSE 1313