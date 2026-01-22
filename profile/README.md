# Make IT Work Cloud

<p align="center">
  <img src="makeitwork-agent.png" alt="Make IT Work Agent" width="300">
</p>

Personal sandbox infrastructure behind [makeitwork.cloud](https://makeitwork.cloud/).

## Repository Map

```
┌─────────────────────────────────────────────────────────────────────────┐
│                              DEPLOY                                     │
├─────────────────────────────────────────────────────────────────────────┤
│  ansible-site-cluster Deploys CRC cluster via ansible-role-crc          │
│  tfroot-aws           AWS S3 buckets, IAM (OpenTofu)                    │
│  tfroot-cloudflare    DNS, tunnels, Zero Trust (OpenTofu)               │
│  tfroot-github        Org settings, repos, teams (OpenTofu)             │
│  tfroot-libvirt       VMs on local libvirt hypervisor (OpenTofu)        │
├─────────────────────────────────────────────────────────────────────────┤
│                             CONFIGURE                                   │
├─────────────────────────────────────────────────────────────────────────┤
│  ansible-project-libvirt Configures libvirt GitHub Actions runners      │
│  kustomize-cluster       GitOps manifests for OpenShift (ArgoCD synced) │
├─────────────────────────────────────────────────────────────────────────┤
│                             WORKLOADS                                   │
├─────────────────────────────────────────────────────────────────────────┤
│  www                 Static website content → S3                        │
├─────────────────────────────────────────────────────────────────────────┤
│                              SHARED                                     │
├─────────────────────────────────────────────────────────────────────────┤
│  ansible-role-crc         Reusable Ansible Role to Deploy OpenShift     │
│  cflan                    Cloudflare LAN utilities for servers          │
│  images                   Container images and shared configs           │
│  shared-workflows         Reusable GitHub Actions workflows             │
│  terraform-libvirt-domain Reusable OpenTofu module for libvirt VMs      │
└─────────────────────────────────────────────────────────────────────────┘
```

## Bootstrap Order

1. `tfroot-aws` → S3 buckets for state backend and web hosting
2. `tfroot-github` → creates repos and org settings
3. `tfroot-cloudflare` → DNS, tunnels, Zero Trust
4. `tfroot-libvirt` → deploy VMs
5. `ansible-project-libvirt` → configure VMs
6. `ansible-site-cluster` → deploy k8s cluster
7. `kustomize-cluster` → configure k8s cluster

## Prerequisites

- [OpenTofu](https://opentofu.org/) or Terraform 1.3+
- [SOPS](https://github.com/getsops/sops) + [age](https://github.com/FiloSottile/age) for secrets
- Ansible 2.9+ with `community.sops` collection
- Cloudflare WARP for private network access

## Get Involved

- **Visit:** [makeitwork.cloud](https://makeitwork.cloud/)
- **Issues:** Open issues on individual repos for suggestions
- **Contribute:** PRs welcome

## License

GPLv3
