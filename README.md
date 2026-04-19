# LaStarter Marketplace

Marketplace officiel pour les extensions LaStarter — Modules et Thèmes.

## À propos

LaStarter est une plateforme multi-tenant modulaire construite sur Laravel 13 + React 19 + TypeScript + Inertia.js. Ce dépôt sert d'index pour les extensions disponibles publiquement.

## Extensions Disponibles

### Modules

| Module | Description | Version |
|--------|-------------|---------|
| [Projects](https://github.com/OneSubnet/lastarter-module-projects) | Gestion de projets avec priorités, dates limite et catégories | 1.0.0 |
| [Tasks](https://github.com/OneSubnet/lastarter-module-tasks) | Gestion de tâches avec liaison aux projets | 1.0.0 |
| [Spaces](https://github.com/OneSubnet/lastarter-module-spaces) | Espaces sécurisés avec documents, signatures et conformité RGPD | 1.0.0 |
| [Forms](https://github.com/OneSubnet/lastarter-module-forms) | Créateur de formulaires avec aperçu en direct et analyse des réponses | 1.0.0 |

### Thèmes

| Thème | Description | Version |
|-------|-------------|---------|
| [Default](https://github.com/OneSubnet/lastarter-theme-default) | Thème par défaut avec dashboard propre et composants UI modernes | 1.0.0 |

## Soumettre une Extension

Pour soumettre votre extension au marketplace :

1. Créez un dépôt GitHub avec le préfixe `lastarter-` (ex: `lastarter-module-monmodule`)
2. Ajoutez le topic `lastarter-extension` à votre dépôt
3. Incluez un fichier `extension.json` valide à la racine
4. Créez une release avec un ZIP contenant votre extension
5. Ouvrez une issue sur ce dépôt avec le lien vers votre extension

### Convention de Nommage

- Modules : `lastarter-module-{slug}`
- Thèmes : `lastarter-theme-{slug}`

### Manifeste (extension.json)

```json
{
    "identifier": "mon-module",
    "name": "Mon Module",
    "type": "module",
    "version": "1.0.0",
    "description": "Description de votre extension",
    "author": "Votre Nom",
    "license": "MIT",
    "provider": "Modules\MonModule\MonModuleServiceProvider",
    "namespace": "Modules\MonModule",
    "lastarterVersion": ">=1.0.0",
    "permissions": [],
    "navigation": {},
    "settings": []
}
```

## Licence

MIT © OneSubnet
