---
page_title: "archil_disk Resource"
description: "Manages an Archil disk."
---

# archil_disk

Manages an Archil disk. Disks are the primary storage unit in Archil, backed by object storage (S3, GCS, R2, S3-compatible, or Azure Blob).

All attributes are immutable — changing any value will destroy and recreate the disk.

## Example Usage

### S3-backed disk

```hcl
resource "archil_disk" "main" {
  name = "my-disk"

  mount {
    type        = "s3"
    bucket_name = "my-archil-bucket"
  }
}
```

### S3-backed disk with IAM role authentication

```hcl
resource "archil_disk" "iam" {
  name = "iam-disk"

  mount {
    type        = "s3"
    bucket_name = "my-archil-bucket"
    session_id  = "asi_custom-session-id"
  }
}
```

### S3-backed disk with access keys

```hcl
resource "archil_disk" "keys" {
  name = "keyed-disk"

  mount {
    type              = "s3"
    bucket_name       = "my-archil-bucket"
    access_key_id     = var.aws_access_key
    secret_access_key = var.aws_secret_key
  }
}
```

### GCS-backed disk

```hcl
resource "archil_disk" "gcs" {
  name = "gcs-disk"

  mount {
    type              = "gcs"
    bucket_name       = "my-gcs-bucket"
    access_key_id     = var.gcs_hmac_key
    secret_access_key = var.gcs_hmac_secret
  }
}
```

### Azure Blob-backed disk

```hcl
resource "archil_disk" "azure" {
  name = "azure-disk"

  mount {
    type                 = "azure-blob"
    container_name       = "my-container"
    storage_account_name = "mystorageaccount"
    tenant_id            = var.azure_tenant_id
    client_id            = var.azure_client_id
    client_secret        = var.azure_client_secret
  }
}
```

## Argument Reference

- `name` (String, Required) - Disk name (1-100 characters, alphanumeric, dashes, and underscores).

### mount Block

Exactly one `mount` block is required.

- `type` (String, Required) - Mount type: `s3`, `gcs`, `r2`, `s3-compatible`, or `azure-blob`.
- `bucket_name` (String) - Bucket name. Required for S3, GCS, R2, and S3-compatible mounts.
- `bucket_endpoint` (String) - Storage endpoint URL. Required for R2 and S3-compatible mounts.
- `access_key_id` (String, Sensitive) - Access key ID.
- `secret_access_key` (String, Sensitive) - Secret access key.
- `session_token` (String, Sensitive) - Session token for temporary credentials.
- `session_id` (String) - Session identifier for IAM role-based authentication. Auto-generated when not set and no access keys are provided.
- `bucket_prefix` (String) - Prefix within the bucket.
- `container_name` (String) - Azure Blob container name.
- `endpoint` (String) - Azure Blob endpoint URL.
- `storage_account_name` (String) - Azure storage account name.
- `tenant_id` (String) - Azure AD tenant ID.
- `client_id` (String) - Azure AD client ID.
- `client_secret` (String, Sensitive) - Azure AD client secret.

## Attribute Reference

- `id` (String) - Disk ID.
- `status` (String) - Disk status.
- `organization` (String) - Owning organization.
- `provider_name` (String) - Cloud provider.
- `region` (String) - Provider region.
- `created_at` (String) - Creation timestamp.
- `data_size` (Number) - Total data size in bytes.

## Import

Disks can be imported by their ID:

```shell
terraform import archil_disk.main <disk-id>
```
