FROM registry.gitlab.com/gitlab-org/terraform-images/stable:latest AS gitlab-terraform
FROM registry.gitlab.com/cmmarslender/kubectl-helm:v4 AS kubectl-helm
FROM hashicorp/packer:latest AS packer
FROM hashicorp/terraform:latest AS terraform
FROM alpine:latest

ENV PACKER_PLUGIN_PATH="/packer-plugins"
ENV PYENV_ROOT=/root/.pyenv
ENV PATH=$PYENV_ROOT/shims:$PYENV_ROOT/bin:$PATH

COPY --from=packer /bin/packer /bin/
COPY --from=terraform /bin/terraform /bin/
COPY --from=kubectl-helm /usr/local/bin/kubectl /bin/
COPY --from=gitlab-terraform /usr/bin/gitlab-terraform /bin/gitlab-terraform
RUN apk add --no-cache ansible aws-cli bash coreutils curl curl-dev git github-cli git go gpg gpg-agent idn2-utils jq make mysql-client netcat-openbsd openssh-client openssh-keygen openssl rsync sudo tar wget xorriso && \
    apk add --no-cache python3 py-pip py-jmespath && \
    apk add --no-cache yq --repository=http://dl-cdn.alpinelinux.org/alpine/edge/community && \
    pip install --no-cache-dir --break-system-packages jinjanator && \
    packer plugins install github.com/hashicorp/amazon && \
    packer plugins install github.com/hashicorp/ansible && \
    packer plugins install github.com/hashicorp/proxmox
