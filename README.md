# LaStarter Marketplace

Marketplace officiel pour les extensions [LaStarter](https://github.com/OneSubnet/LaStarter) — Modules et Themes.

## A propos

LaStarter est une plateforme multi-tenant modulaire construite sur **Laravel 13 + React 19 + TypeScript + Inertia.js 3**. Ce depot sert d'index pour les extensions disponibles publiquement, integrees via le systeme de marketplace GitHub.

## Structure

```
modules/{owner}/{repo}/   — Extensions module
themes/{owner}/{repo}/    — Extensions theme
```

## Extensions Disponibles

### Themes

| Theme | Description | Version |
|-------|-------------|---------|
| Default | Theme par defaut avec dashboard propre et composants UI modernes | 1.0.0 |

## Soumettre une Extension

Pour soumettre votre extension au marketplace :

1. Creez un depot GitHub public avec un `extension.json` valide a la racine
2. Ajoutez le topic `lastarter-extension` a votre depot
3. Creez une release GitHub avec un ZIP contenant votre extension
4. Ouvrez une issue sur ce depot avec le lien vers votre depot

### Structure d'un Module

```
{slug}/
├── extension.json              # Manifeste obligatoire
├── src/
│   ├── Providers/
│   │   └── {Name}ServiceProvider.php
│   ├── Controllers/
│   ├── Models/                 # Avec le trait HasTeam
│   ├── Policies/               # Avec hasPermissionTo()
│   └── Http/Requests/
├── routes/
│   └── web.php
├── database/
│   └── migrations/
└── resources/
    ├── js/pages/               # Pages Inertia React
    └── locales/
        ├── en.json
        └── fr.json
```

### Manifeste (extension.json)

Le manifeste doit suivre le schema JSON officiel :

```json
{
  "$schema": "https://raw.githubusercontent.com/one-subnet/lastarter/refs/heads/main/schemas/extension.schema.json",
  "name": "Mon Module",
  "identifier": "mon-module",
  "type": "module",
  "version": "1.0.0",
  "description": "Description de votre extension",
  "author": "Votre Nom",
  "minimum_core_version": "^1.0",
  "namespace": "Modules\MonModule",
  "provider": "Modules\MonModule\Providers\MonModuleServiceProvider",
  "dependencies": [],
  "provides": ["mon-feature"],
  "permissions": [
    "mon-module.view",
    "mon-module.create",
    "mon-module.update",
    "mon-module.delete"
  ],
  "navigation": {
    "app": [
      {
        "title": "Mon Module",
        "icon": "FolderKanban",
        "order": 10,
        "children": [
          {
            "title": "Liste",
            "route": "mon-module.index",
            "icon": "List",
            "order": 1
          }
        ]
      }
    ]
  },
  "widgets": [],
  "metrics": []
}
```

### Champs du Manifeste

| Champ | Requis | Description |
|------|--------|--------------|
| `name` | Oui | Nom d'affichage |
| `identifier` | Oui | Identifiant unique (slug) |
| `type` | Oui | `module`, `theme` ou `language` |
| `version` | Oui | Version semver |
| `description` | Oui | Description courte |
| `author` | Oui | Auteur ou organisation |
| `minimum_core_version` | Non | Contrainte de version du core (ex: `^1.0`) |
| `namespace` | Oui | Namespace PHP racine |
| `provider` | Oui | Classe ServiceProvider (doit etendre `ModuleServiceProvider`) |
| `dependencies` | Non | Identifiants des extensions requises |
| `provides` | Non | Fonctionnalites fournies (identifiants) |
| `permissions` | Oui | Liste des permissions declarees |
| `navigation` | Non | Entrees de sidebar (avec icones Lucide, routes, ordre) |
| `widgets` | Non | Widgets dashboard (type `stat` ou `list`) |
| `metrics` | Non | Metriques exposees pour les tableaux de bord |

### Types d'Extensions

| Type | Description |
|------|-------------|
| `module` | Logique metier (CRM, facturation, projets...) |
| `theme` | Surcharges UI, CSS et composants |
| `language` | Packs de traduction supplementaires |

### Regles de Compatibilite

- `provides` et `permissions` ne peuvent que **grandir** entre les versions — jamais diminuer
- Le `CompatibilityChecker` bloque les mises a jour qui violent ce contrat
- `minimum_core_version` est verifie a l'installation

### Regles de Securite

- **Toujours** utiliser `$user->hasPermissionTo()` pour les autorisations
- **Toujours** utiliser le trait `HasTeam` sur les modeles
- Les routes de module sont automatiquement protegees par `EnsureExtensionEnabled`
- Les pages de module utilisent les imports `@/` (resolus par Vite)
- Fournir les traductions dans `resources/locales/{locale}.json`

## Licence

MIT — OneSubnet
