# terraform-hcloud-server

> One server, wired to its keys, network and disks — not a bare VM you still have to plumb.

**Status:** 🚧 In development

## Overview

Terraform module that manages a Hetzner Cloud server. It creates a single server from an image and server type, attaches SSH keys, an optional private network and volumes, and exposes the public and private IPv4 addresses.

## Features

- Creates one `hcloud_server` from an image, server type and location or datacenter
- Attaches existing SSH keys by name or ID, so no password is ever mailed out
- Optional private network attachment with an explicitly chosen or pool-assigned alias IP
- Attaches existing volumes and lets Terraform own the mount decision, not cloud-init guesswork
- Outputs `ipv4_address` and `private_ipv4_address` separately, so downstream modules never guess which one they got
- `user_data` and labels passed straight through for cloud-init and inventory selectors

## Stack

Terraform + the hetznercloud/hcloud provider.

## Usage

```hcl
module "app_server" {
  source = "github.com/moveeeax/terraform-hcloud-server"

  name         = "app-01"
  server_type  = "cx22"
  image        = "debian-12"
  location     = "nbg1"
  ssh_keys     = ["ops-team"]

  network_id         = module.network.network_id
  private_ipv4       = "10.0.1.10"
  volume_ids         = [module.data_volume.id]

  labels = {
    role = "app"
    env  = "prod"
  }
}
```

## License

MIT
