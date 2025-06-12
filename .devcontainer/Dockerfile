FROM alpine:latest

# Install basic utilities
RUN apk add --no-cache bash curl git

# Install GitHub CLI
RUN apk add --no-cache openssh && \
    curl -fsSL https://github.com/cli/cli/releases/latest/download/gh_$(uname -m | sed 's/x86_64/amd64/;s/aarch64/arm64/').apk -o /tmp/gh.apk && \
    apk add --allow-untrusted /tmp/gh.apk && \
    rm /tmp/gh.apk

# Install Talos CLI
RUN curl -Lo /usr/local/bin/talosctl https://github.com/siderolabs/talos/releases/latest/download/talosctl-$(uname -s | tr '[:upper:]' '[:lower:]')-amd64 && \
    chmod +x /usr/local/bin/talosctl

# Install AWS CLI v2
RUN apk add --no-cache python3 py3-pip && \
    pip3 install --upgrade pip && \
    pip3 install awscli

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -Ls https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
    install -m 0755 kubectl /usr/local/bin/kubectl && \
    rm kubectl

# Set default shell
SHELL ["/bin/bash", "-c"]
