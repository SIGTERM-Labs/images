# Images

This repository just bakes some Docker images on a schedule.

## IaC

This image is for infrastructure as code automation in CI jobs. It ships:

- Provisioning: `terraform`, `gitlab-terraform`, `packer` (with the `amazon`,
  `ansible` and `proxmox` plugins baked in), `ansible`, `talosctl`
- Cloud and cluster CLIs: `aws-cli`, `kubectl`
- Templating and parsing: `j2`, `jq`, `yq`, `python3` with `jmespath`
- General tooling: `bash`, `coreutils`, `curl`, `git`, `github-cli`, `go`, `gpg`,
  `make`, `mysql-client`, `netcat-openbsd`, `openssh-client`, `openssl`, `rsync`,
  `sudo`, `tar`, `wget`, `xorriso`

It is published to `ghcr.io/SIGTERM-Labs/images/iac`.

### Vault variant

Every tag is also published with a `-vault` suffix, which is the same image plus
the HashiCorp `vault` CLI. Use it when a job needs to talk to Vault, and use the
plain tag otherwise:

```
ghcr.io/SIGTERM-Labs/images/iac:latest
ghcr.io/SIGTERM-Labs/images/iac:latest-vault
```
