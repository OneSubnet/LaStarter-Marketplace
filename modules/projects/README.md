# Projects Module

A project management module for LaStarter that lets teams organize their work into projects.

## Features

- Create and manage projects within your team
- Custom visibility settings (private, team, public)
- Permission-based access control
- Integrates with the Tasks module

## Permissions

```
project.view    # View projects
project.create   # Create new projects
project.update   # Update projects
project.delete   # Delete projects
```

## Installation

Install through the marketplace or run:

```bash
php artisan extensions:scan
php artisan extensions:sync
php artisan extensions:enable projects
php artisan migrate
```

## Settings

| Setting | Description | Default |
|---------|-------------|---------|
| Default Visibility | New project visibility | private |
