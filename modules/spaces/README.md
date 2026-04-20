# Spaces Module

Secure workspace management with document handling, e-signatures, and RGPD compliance for organizations.

## Features

- Create secure workspaces for teams and clients
- Document upload and management
- E-signature request and validation
- RGPD-compliant data retention policies
- User account deletion workflows
- Permission-based access per workspace

## Permissions

```
space.view                # View workspaces
space.create              # Create new workspaces
space.update              # Update workspaces
space.delete              # Delete workspaces
space.manage-members      # Manage workspace members
document.view             # View documents
document.upload           # Upload documents
document.delete           # Delete documents
document.request-signature # Request e-signatures
document.sign             # Sign documents
```

## Installation

Install through the marketplace or run:

```bash
php artisan extensions:scan
php artisan extensions:sync
php artisan extensions:enable spaces
php artisan migrate
```

## Settings

| Setting | Description | Default |
|---------|-------------|---------|
| Max Upload Size (MB) | Maximum document file size | 25 |
| Require Signature | Signature requirement level | optional |
| RGPD Retention (days) | Data retention period | 365 |
| Allow Account Deletion | User self-service deletion | true |
