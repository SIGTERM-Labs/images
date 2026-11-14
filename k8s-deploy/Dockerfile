FROM hashicorp/vault:latest AS vault
FROM ghcr.io/helmfile/helmfile:latest AS helmfile
FROM registry.k8s.io/kubectl:v1.37.2 AS kubectl
FROM mikefarah/yq:4 AS yq
FROM alpine:latest AS k8s-deploy
ARG HELM_VERSION

COPY --from=kubectl /bin/kubectl /bin/
COPY --from=helmfile /usr/local/bin/helmfile /bin/
COPY --from=yq /usr/bin/yq /usr/bin/

RUN apk add --no-cache bash curl git jq openssl openssh-client py3-pip python3 && \
    pip install --no-cache-dir --break-system-packages jinjanator

RUN curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4 && \
    bash get_helm.sh --version $HELM_VERSION

FROM k8s-deploy AS k8s-deploy-vault

COPY --from=vault /bin/vault /bin/
