# Forms Module

Create custom forms with live preview, collect responses, and analyze results — like Google Forms for your team.

## Features

- Visual form builder with live preview
- Multiple field types (text, email, select, textarea, etc.)
- Public and private form sharing
- Response collection and analytics
- File upload support
- Notification on submission

## Permissions

```
form.view                # View forms
form.create              # Create new forms
form.update              # Update forms
form.delete              # Delete forms
form.publish             # Publish/unpublish forms
form-submission.view     # View form responses
form-submission.create   # Submit form responses
form-submission.delete   # Delete form responses
```

## Installation

Install through the marketplace or run:

```bash
php artisan extensions:scan
php artisan extensions:sync
php artisan extensions:enable forms
php artisan migrate
```

## Settings

| Setting | Description | Default |
|---------|-------------|---------|
| Max File Upload (MB) | Maximum file upload size | 10 |
| Require Login | Must be logged in to submit | false |
| Default Notification | Notification on new submission | none |
