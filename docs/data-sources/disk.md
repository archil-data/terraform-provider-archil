---
page_title: "archil_disk Data Source"
description: "Look up an Archil disk by ID or name."
---

# archil_disk (Data Source)

Look up an existing Archil disk by ID or name. Exactly one of `id` or `name` must be set.

## Example Usage

### Look up by name

```hcl
data "archil_disk" "prod" {
  name = "production-disk"
}

output "disk_id" {
  value = data.archil_disk.prod.id
}
```

### Look up by ID

```hcl
data "archil_disk" "existing" {
  id = "disk-abc123"
}
```

## Argument Reference

- `id` (String) - Disk ID. Exactly one of `id` or `name` must be set.
- `name` (String) - Disk name. Exactly one of `id` or `name` must be set.

## Attribute Reference

- `id` (String) - Disk ID.
- `name` (String) - Disk name.
- `status` (String) - Disk status.
- `organization` (String) - Owning organization.
- `provider_name` (String) - Cloud provider.
- `region` (String) - Provider region.
- `created_at` (String) - Creation timestamp.
- `data_size` (Number) - Total data size in bytes.
