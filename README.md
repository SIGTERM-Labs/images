# Images

This repository just bakes some Docker images on a schedule. Both are published
for `linux/amd64` and `linux/arm64`, and each has a `-vault` variant (see below).

| Image | Use it for |
|---|---|
| [`iac`](#iac) | Building infrastructure and clusters |
| [`k8s-deploy`](#k8s-deploy) | Deploying to an existing Kubernetes cluster |

## IaC

This image is for infrastructure as code automation in CI jobs, including
building clusters. It ships:

- Provisioning: `terraform`, `gitlab-terraform`, `packer` (with the `amazon`,
  `ansible` and `proxmox` plugins baked in), `ansible`, `talosctl`
- Cloud and cluster CLIs: `aws-cli`, `kubectl`, `cilium`
- Templating and parsing: `jinjanate`, `jq`, `yq`, `python3` with `jmespath`
- General tooling: `bash`, `coreutils`, `curl`, `git`, `github-cli`, `go`, `gpg`,
  `make`, `mysql-client`, `netcat-openbsd`, `openssh-client`, `openssl`, `rsync`,
  `sudo`, `tar`, `wget`, `xorriso`

It is published to `ghcr.io/SIGTERM-Labs/images/iac`.

## k8s-deploy

This image is for deploying to Kubernetes in CI jobs. It holds only what a
deploy needs, so it stays small. It ships:

- Kubernetes: `kubectl`, `helm` (v4), `helmfile`
- Templating and parsing: `jsonnet`, `jb`, `gojsontoyaml`, `jinjanate`, `jq`, `yq`
- General tooling: `bash`, `curl`, `git`, `openssh-client`, `openssl`, `python3`

It is published to `ghcr.io/SIGTERM-Labs/images/k8s-deploy`.

## Vault variant

Every tag of both images is also published with a `-vault` suffix, which is the
same image plus the HashiCorp `vault` CLI. Use it when a job needs to talk to
Vault, and use the plain tag otherwise:

```
ghcr.io/SIGTERM-Labs/images/iac:latest
ghcr.io/SIGTERM-Labs/images/iac:latest-vault
ghcr.io/SIGTERM-Labs/images/k8s-deploy:latest
ghcr.io/SIGTERM-Labs/images/k8s-deploy:latest-vault
```
