---
page_title: "archil_api_token Resource"
description: "Manages an Archil API token."
---

# archil_api_token

Manages an Archil API token. Tokens are used to authenticate users to the Archil control plane API.

The full token value is only available at creation time. All attributes are immutable — changing any value will destroy and recreate the token.

## Example Usage

```hcl
resource "archil_api_token" "ci" {
  name        = "ci-deploy"
  description = "Token for CI/CD pipelines"
}

output "token" {
  value     = archil_api_token.ci.token
  sensitive = true
}
```

## Argument Reference

- `name` (String, Required) - Token name (1-100 characters).
- `description` (String) - Token description (max 500 characters).

## Attribute Reference

- `id` (String) - Token ID (hash).
- `token_suffix` (String) - Last 4 characters of the token.
- `token` (String, Sensitive) - Full token value. Only available at creation.
- `created_at` (String) - Creation timestamp.
