FROM registry.gitlab.com/gitlab-org/terraform-images/stable:latest AS gitlab-terraform
FROM registry.k8s.io/kubectl:v1.37.2 AS kubectl
FROM mikefarah/yq:4 AS yq
FROM hashicorp/packer:latest AS packer
FROM hashicorp/terraform:latest AS terraform
FROM hashicorp/vault:latest AS vault
FROM alpine:latest AS iac

ENV PACKER_PLUGIN_PATH="/packer-plugins"
ENV PYENV_ROOT=/root/.pyenv
ENV PATH=$PYENV_ROOT/shims:$PYENV_ROOT/bin:$PATH

COPY --from=packer /bin/packer /bin/
COPY --from=terraform /bin/terraform /bin/
COPY --from=kubectl /bin/kubectl /bin/
COPY --from=yq /usr/bin/yq /usr/bin/
COPY --from=gitlab-terraform /usr/bin/gitlab-terraform /bin/gitlab-terraform

# Set by BuildKit for the platform being built: amd64 or arm64
ARG TARGETARCH

RUN apk add --no-cache ansible aws-cli bash coreutils curl curl-dev git github-cli go gpg gpg-agent idn2-utils jq make mysql-client netcat-openbsd openssh-client openssh-keygen openssl rsync sudo tar wget xorriso && \
    apk add --no-cache python3 py-pip py-jmespath && \
    pip install --no-cache-dir --break-system-packages jinjanator

RUN curl -fsSLo /bin/talosctl "https://github.com/siderolabs/talos/releases/latest/download/talosctl-linux-${TARGETARCH}" && \
    chmod +x /bin/talosctl

RUN CILIUM_CLI_VERSION=$(curl -fsSL https://raw.githubusercontent.com/cilium/cilium-cli/main/stable.txt) && \
    cd /tmp && \
    curl -fsSL --remote-name-all "https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-linux-${TARGETARCH}.tar.gz{,.sha256sum}" && \
    sha256sum -c "cilium-linux-${TARGETARCH}.tar.gz.sha256sum" && \
    tar xzf "cilium-linux-${TARGETARCH}.tar.gz" -C /usr/local/bin cilium && \
    rm -f "cilium-linux-${TARGETARCH}.tar.gz" "cilium-linux-${TARGETARCH}.tar.gz.sha256sum"

RUN packer plugins install github.com/hashicorp/amazon && \
    packer plugins install github.com/hashicorp/ansible && \
    packer plugins install github.com/hashicorp/proxmox

FROM iac AS iac-vault

COPY --from=vault /bin/vault /bin/
