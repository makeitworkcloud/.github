# Make IT Work Cloud

Personal sandbox infrastructure behind [makeitwork.cloud](https://makeitwork.cloud/).

## Repository Map

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           INFRASTRUCTURE                                │
├─────────────────────────────────────────────────────────────────────────┤
│  tfroot-aws          AWS S3 buckets, IAM (OpenTofu)                     │
│  tfroot-cloudflare   DNS, tunnels, Zero Trust (OpenTofu)                │
│  tfroot-github       Org settings, repos, teams (OpenTofu)              │
│  tfroot-libvirt      VMs on local libvirt hypervisor (OpenTofu)         │
├─────────────────────────────────────────────────────────────────────────┤
│                           CONFIGURATION                                 │
├─────────────────────────────────────────────────────────────────────────┤
│  ansible-site-cluster    Deploys CRC cluster via ansible-role-crc      │
│  ansible-role-crc        OpenShift Local + ArgoCD + KSOPS              │
│  ansible-project-libvirt Configures libvirt GitHub Actions runners     │
├─────────────────────────────────────────────────────────────────────────┤
│                           WORKLOADS                                     │
├─────────────────────────────────────────────────────────────────────────┤
│  kustomize-cluster   GitOps manifests for OpenShift (ArgoCD synced)    │
│  www                 Static website content → S3                        │
├─────────────────────────────────────────────────────────────────────────┤
│                           SHARED                                        │
├─────────────────────────────────────────────────────────────────────────┤
│  terraform-libvirt-domain   Reusable OpenTofu module for libvirt VMs   │
│  shared-workflows           Reusable GitHub Actions workflows          │
│  images                     Container images and shared configs        │
│  cflan                      Cloudflare LAN utilities                   │
└─────────────────────────────────────────────────────────────────────────┘
```

## Bootstrap Order

1. `tfroot-aws` → S3 buckets for state backend and web hosting
2. `tfroot-github` → creates repos and org settings
3. `tfroot-cloudflare` → DNS, tunnels, Zero Trust
4. `tfroot-libvirt` → provision VMs
5. `ansible-project-libvirt` → configure GitHub runners
6. `ansible-site-cluster` → deploy CRC cluster
7. `kustomize-cluster` → ArgoCD syncs workloads

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
