# Terraform Provider for Archil

The Archil Terraform provider lets you manage [Archil](https://archil.com) disks, users, and API tokens as infrastructure-as-code.

Full documentation is available at [docs.archil.com](https://docs.archil.com).

## Setup

```hcl
terraform {
  required_providers {
    archil = {
      source = "archil-data/archil"
    }
  }
}

provider "archil" {
  api_key = var.archil_api_key  # or set ARCHIL_API_KEY
  region  = "us-east-1"
}
```

## Examples

### Create a disk with S3 storage

```hcl
resource "archil_disk" "main" {
  name = "my-disk"

  mount {
    type        = "s3"
    bucket_name = "my-archil-bucket"
  }
}
```

### Add a user to a disk

```hcl
resource "archil_api_token" "ci" {
  name        = "ci-deploy"
  description = "Token for CI/CD pipelines"
}

resource "archil_disk_user" "ci" {
  disk_id   = archil_disk.main.id
  type      = "token"
  principal = archil_api_token.ci.token
  nickname  = "ci-deploy"
}
```

### Look up an existing disk

```hcl
data "archil_disk" "existing" {
  name = "production-disk"
}
```

## Issues

Found a bug or have a feature request? [Open an issue](https://github.com/archil-data/terraform-provider-archil/issues).
