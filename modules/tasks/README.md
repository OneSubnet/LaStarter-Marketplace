# Tasks Module

A simple task management module for LaStarter with project linking capabilities.

## Features

- Create, assign, and track tasks
- Link tasks to projects (requires Projects module)
- Status workflow: To Do, In Progress, Done
- Permission-based access control

## Permissions

```
task.view      # View tasks
task.create    # Create new tasks
task.update    # Update tasks
task.delete    # Delete tasks
```

## Installation

Install through the marketplace or run:

```bash
php artisan extensions:scan
php artisan extensions:sync
php artisan extensions:enable tasks
php artisan migrate
```

## Settings

| Setting | Description | Default |
|---------|-------------|---------|
| Default Status | New task status | todo |
