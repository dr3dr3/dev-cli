ARG ALPINE_VERSION=latest
FROM alpine:${ALPINE_VERSION}

LABEL maintainer="Andre Dreyer"

# Install basic utilities
RUN apk add --no-cache bash bash-completion curl git gcompat openssl figlet
RUN git config --global user.name "Andre Dreyer" && git config --global user.email "git@andredreyer.com"

# Install GitHub CLI
RUN apk add --no-cache github-cli

# Install Talosctl
RUN curl -sL https://talos.dev/install | sh
RUN alias t=talosctl

# Install Kubectl
# Useful aliases for kubectl: https://github.com/ahmetb/kubectl-aliases/blob/master/.kubectl_aliases
RUN apk add --no-cache kubectl && \
    alias k='kubectl' && \
    alias kgp='kubectl get pods' && \
    alias ka='kubectl apply'

# Install Kubectl Krew plugin manager
RUN set -x; cd "$(mktemp -d)" && \
    OS="$(uname | tr '[:upper:]' '[:lower:]')" && \
    ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" && \
    KREW="krew-${OS}_${ARCH}" && \
    curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" && \
    tar zxvf "${KREW}.tar.gz" && \
    ./"${KREW}" install krew

# Set up PATH for krew
ENV PATH="/root/.krew/bin:${PATH}"

# Install Krew plugins
# https://github.com/ahmetb/kubectx
# https://github.com/yonahd/kor
# https://github.com/itaysk/kubectl-neat
# https://github.com/zegl/kube-score
RUN kubectl krew install ctx && \
    kubectl krew install ns && \
    kubectl krew install kor && \
    kubectl krew install neat && \
    kubectl krew install score

ENTRYPOINT [ "/bin/bash" ]
