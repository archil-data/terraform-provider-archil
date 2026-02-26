---
page_title: "archil_disk_user Resource"
description: "Manages an authorized user on an Archil disk."
---

# archil_disk_user

Manages an authorized user on an Archil disk. Users can authenticate via API token or AWS STS (IAM role).

All attributes are immutable — changing any value will destroy and recreate the user.

## Example Usage

### Token-based user

```hcl
resource "archil_api_token" "app" {
  name = "my-app"
}

resource "archil_disk_user" "app" {
  disk_id   = archil_disk.main.id
  type      = "token"
  principal = archil_api_token.app.token
  nickname  = "my-app"
}
```

### AWS STS (IAM role) user

```hcl
resource "archil_disk_user" "iam" {
  disk_id   = archil_disk.main.id
  type      = "awssts"
  principal = "arn:aws:iam::123456789012:role/my-role"
}
```

## Argument Reference

- `disk_id` (String, Required) - Disk ID to add the user to.
- `type` (String, Required) - User type: `token` or `awssts`.
- `principal` (String, Required, Sensitive) - Token value or IAM ARN.
- `nickname` (String) - Nickname for the user. Required when `type = "token"`.

## Attribute Reference

- `token_suffix` (String) - Last 4 characters of the token.
- `created_at` (String) - Creation timestamp.
