---
page_title: "Archil Provider"
description: "The Archil provider lets you manage disks, users, and API tokens on the Archil distributed filesystem."
---

# Archil Provider

The Archil provider lets you manage [Archil](https://archil.com) disks, users, and API tokens as infrastructure-as-code.

Full documentation is available at [docs.archil.com](https://docs.archil.com).

## Authentication

The provider requires an API key, which can be set in the provider configuration or via the `ARCHIL_API_KEY` environment variable.

```hcl
provider "archil" {
  api_key = var.archil_api_key
  region  = "aws-us-east-1"
}
```

## Example Usage

```hcl
terraform {
  required_providers {
    archil = {
      source = "archil-data/archil"
    }
  }
}

provider "archil" {
  region = "aws-us-east-1"
}

resource "archil_disk" "main" {
  name = "my-disk"

  mount {
    type        = "s3"
    bucket_name = "my-archil-bucket"
  }
}

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

## Argument Reference

- `api_key` (String, Sensitive) - API key for Archil. Can also be set via `ARCHIL_API_KEY`.
- `region` (String) - Archil region. Can also be set via `ARCHIL_REGION`. Default: `aws-us-east-1`. Valid regions: `aws-us-east-1`, `aws-eu-west-1`, `aws-us-west-2`, `gcp-us-central1`.
- `endpoint` (String) - Override the API endpoint URL. Can also be set via `ARCHIL_ENDPOINT`.
