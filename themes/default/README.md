# Default Theme

The default LaStarter theme with a brutalist design — bold red primary, black borders, hard shadows, and Inter font.

## Features

- Brutalist design language with hard offset shadows
- Red primary color with black/white contrast
- Inter font family
- No border radius (sharp edges)
- Dark mode support with inverted color scheme
- Clean dashboard with widget cards

## Design Tokens

| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| Primary | `hsl(0, 100%, 43%)` | `oklch(0.628 0.258 29.2)` |
| Background | White | Black |
| Border | Black | White |
| Shadow | 3px 3px 0px black | 3px 3px 0px white |
| Radius | 0px | 0px |
| Font | Inter | Inter |

## Installation

Install through the marketplace or run:

```bash
php artisan extensions:scan
php artisan extensions:sync
php artisan extensions:enable default
```

Only one theme can be active per team. Enabling this theme will deactivate the current theme.
