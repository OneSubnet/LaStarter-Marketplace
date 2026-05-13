# LaStarter Marketplace

Marketplace officiel pour les extensions [LaStarter](https://github.com/OneSubnet/LaStarter) — Modules, Thèmes et Langues.

## À propos

LaStarter est une plateforme multi-tenant modulaire construite sur **Laravel 13 + React 19 + TypeScript + Inertia.js 3**. Ce dépôt sert d'index pour les extensions disponibles publiquement, intégrées via le système de marketplace GitHub.

Le thème par défaut est directement intégré à l'application — il n'est pas distribué séparément.

## Stack technique

| Couche | Technologie |
|--------|-------------|
| Backend | Laravel 13, PHP 8.4+ |
| Frontend | React 19, TypeScript, Inertia.js 3 |
| UI | Tailwind CSS v4, Radix UI (shadcn/ui), icônes Lucide |
| Auth/Rôles | Laravel Fortify, Spatie Permission (mode teams) |
| Temps réel | Laravel Reverb + Echo |
| i18n | i18next + react-i18next (EN/FR) |
| Tests | Pest 4, PHPStan, ESLint, Prettier |
| CI | GitHub Actions (lint, tests PHP 8.4/8.5 × SQLite/PG, release) |

## Structure

```
modules/{owner}/{repo}/   — Extension module
themes/{owner}/{repo}/    — Extension thème
```

## Soumettre une extension

Pour soumettre votre extension au marketplace :

1. Créez un dépôt GitHub public avec un `extension.json` valide à la racine
2. Ajoutez le topic `lastarter-extension` à votre dépôt
3. Créez une release GitHub avec un ZIP contenant votre extension
4. Ouvrez une issue sur ce dépôt avec le lien vers votre dépôt

---

## Créer un module

### En une commande

```bash
php artisan extensions:make blog
```

Cette commande scaffold un module complet :

```
extensions/modules/blog/
├── extension.json                    # Manifeste
├── src/
│   └── BlogServiceProvider.php       # Fournisseur de services
├── routes/
│   └── web.php                       # Routes auto-enregistrées
├── database/
│   └── migrations/                   # Migrations avec team_id
├── resources/
│   ├── js/pages/blog/
│   │   └── Index.tsx                 # Page d'accueil du module
│   └── locales/
│       ├── en.json                   # Traductions anglaises
│       └── fr.json                   # Traductions françaises
└── tests/
```

Puis activez le module :

```bash
php artisan extensions:scan          # Enregistrer en base
php artisan extensions:sync          # Synchroniser les permissions
php artisan extensions:install blog  # Lancer les migrations
php artisan extensions:enable blog --team=1  # Activer pour une équipe
```

### Générer des ressources

Comme avec NestJS CLI, chaque ressource peut être ajoutée individuellement :

```bash
php artisan make:module:model blog Post         # Modèle (HasTeam) + migration
php artisan make:module:controller blog PostController  # Contrôleur Inertia
php artisan make:module:policy blog Post        # Policy avec hasPermissionTo()
php artisan make:module:page blog Show          # Page React TypeScript
php artisan make:module:request blog StorePost  # Form Request
php artisan make:module:service blog PostService # Service class
php artisan make:module:migration blog add_slug # Migration avec team_id
php artisan make:module:test blog Post          # Test Pest

# Ou interactivement :
php artisan make:extension:resource
```

Les stubs peuvent être personnalisés :

```bash
php artisan extension:stubs:publish   # Publie les stubs dans stubs/extensions/
```

---

## Créer un thème

```bash
php artisan make:theme mon-theme
```

Structure générée :

```
extensions/themes/mon-theme/
├── extension.json
├── src/
│   └── MonThemeServiceProvider.php
└── resources/
    └── js/
        └── overrides/     # Placez vos surcharges de pages ici
```

### Surcharger une page

Un thème peut surcharger n'importe quelle page (core ou module) en plaçant un fichier dans `resources/js/overrides/` :

```
overrides/Dashboard.tsx       → Surcharge la page Dashboard du core
overrides/blog/Index.tsx      → Surcharge la page d'index du module blog
```

La résolution suit 3 niveaux : **thème → module → core**.

---

## Le manifeste `extension.json`

Chaque extension doit contenir un fichier `extension.json` à sa racine.

### Exemple complet

```json
{
    "identifier": "blog",
    "name": "Blog",
    "type": "module",
    "version": "1.0.0",
    "description": "Module de gestion de blog",
    "author": "Votre Nom",
    "minimum_core_version": "^1.0",
    "namespace": "Extensions\\Modules\\Blog",
    "provider": "Extensions\\Modules\\Blog\\BlogServiceProvider",
    "dependencies": [],
    "provides": ["blog.posts"],
    "permissions": [
        "blog.view",
        "blog.create",
        "blog.update",
        "blog.delete"
    ],
    "navigation": {
        "app": [
            {
                "title": "Blog",
                "icon": "FileText",
                "order": 10,
                "children": [
                    {
                        "title": "Articles",
                        "route": "blog.index",
                        "icon": "List",
                        "order": 1
                    },
                    {
                        "title": "Nouveau",
                        "route": "blog.create",
                        "icon": "Plus",
                        "order": 2
                    }
                ]
            }
        ]
    },
    "widgets": [],
    "metrics": []
}
```

### Référence des champs

| Champ | Requis | Description |
|-------|--------|-------------|
| `identifier` | Oui | Identifiant unique (slug : minuscules, tirets) |
| `name` | Oui | Nom d'affichage |
| `type` | Oui | `module`, `theme` ou `language` |
| `version` | Oui | Version semver |
| `description` | Oui | Description courte |
| `author` | Oui | Auteur ou organisation |
| `minimum_core_version` | Non | Contrainte de version du core (ex: `^1.0`) |
| `namespace` | Oui | Namespace PHP racine (PSR-4) |
| `provider` | Oui | Classe ServiceProvider (doit étendre `ModuleServiceProvider`) |
| `dependencies` | Non | Identifiants des extensions requises |
| `provides` | Non | Fonctionnalités fournies (identifiants) |
| `permissions` | Oui | Liste des permissions déclarées |
| `navigation` | Non | Entrées de sidebar (icônes Lucide, routes, ordre) |
| `widgets` | Non | Widgets dashboard (type `stat` ou `list`) |
| `metrics` | Non | Métriques exposées pour les tableaux de bord |

### Types d'extensions

| Type | Description |
|------|-------------|
| `module` | Logique métier (CRM, facturation, projets...) |
| `theme` | Surcharges UI, CSS et composants |
| `language` | Packs de traduction supplémentaires |

---

## Architecture

### Multi-tenance

La **Team = Organisation**. Le modèle `Team` est le tenant. Pas de modèle `Organization` séparé.

- **Global Scope** : Le trait `HasTeam` + `TeamScope` filtre automatiquement toutes les requêtes par `current_team_id`
- **Spatie teams mode** : Les rôles sont scoping par team via `team_id` sur la table `roles`
- **TeamRole** : `Owner`, `Admin`, `Member`. Le rôle `owner` est protégé
- Les permissions sont globales (pas par team). Les rôles sont par team

### Modèles avec HasTeam

Chaque modèle de module **doit** utiliser le trait `HasTeam`. Le CLI le génère automatiquement :

```php
use App\Concerns\HasTeam;
use Illuminate\Database\Eloquent\Model;

final class Post extends Model
{
    use HasTeam;

    protected $table = 'blog_posts';
    protected $fillable = ['title', 'content'];
}
```

Cela garantit :
- Filtrage automatique par `team_id` sur toutes les requêtes
- Remplissage automatique de `team_id` à la création
- Isolation complète des données entre équipes

### Autorisations avec les Policies

Les policies utilisent **toujours** `$user->hasPermissionTo()`, jamais de vérification de rôle :

```php
use App\Models\User;

final class PostPolicy
{
    public function viewAny(User $user): bool
    {
        return $user->hasPermissionTo('blog.view');
    }

    public function create(User $user): bool
    {
        return $user->hasPermissionTo('blog.create');
    }

    public function update(User $user, Post $post): bool
    {
        return $user->hasPermissionTo('blog.update');
    }

    public function delete(User $user, Post $post): bool
    {
        return $user->hasPermissionTo('blog.delete');
    }
}
```

N'oubliez pas d'enregistrer votre policy dans `AppServiceProvider::boot()` :

```php
Gate::policy(Post::class, PostPolicy::class);
```

### Pages React

Les pages utilisent Inertia.js avec les imports `@/` :

```tsx
import { Head } from '@inertiajs/react';
import { useTranslation } from 'react-i18next';
import AppLayout from '@/layouts/app-layout';
import type { SharedData } from '@/types';

export default function BlogIndex() {
    const { t } = useTranslation();

    return (
        <AppLayout breadcrumbs={[{ title: t('blog.breadcrumb'), href: '/' }]}>
            <Head title={t('blog.title')} />
            <div>
                <p>Blog</p>
            </div>
        </AppLayout>
    );
}
```

Pour masquer/afficher des éléments selon les permissions :

```tsx
import Guard from '@/components/guard';

<Guard permission="blog.create">
    <button>Créer un article</button>
</Guard>
```

### Communication inter-modules

Les modules peuvent communiquer via `ModuleApiRegistry` :

```php
// Exposer une API dans votre ServiceProvider
ModuleApiRegistry::registerApi(
    ContactApiInterface::class,
    'crm',
    ContactApi::class
);

// Consommer une API depuis un autre module
$api = ModuleApiRegistry::get(ContactApiInterface::class);
$contacts = $api->search('john');
```

### Hooks

Le système de hooks permet de réagir aux événements du core et des extensions :

```php
use App\Core\Hooks\Hook;

// Écouter un hook
Hook::listen(Hook::SIDEBAR_BUILD, function ($navigation) {
    $navigation->addItem([
        'title' => 'Mon Module',
        'icon' => 'FolderKanban',
        'route' => 'mon-module.index',
    ]);
    return $navigation;
});
```

Hooks disponibles : `SIDEBAR_BUILD`, `MODULE_BOOT`, `EXTENSION_ENABLED`, `EXTENSION_DISABLED`, `EXTENSION_INSTALLED`, `EXTENSION_UNINSTALLED`, `EXTENSION_ERROR`, `THEME_CHANGED`

### Settings par team

```php
setting('blog.posts_per_page');              // Lecture
setting_set('blog.posts_per_page', 10);      // Écriture
```

### i18n

Placez vos traductions dans `resources/locales/{locale}.json` :

```json
{
    "blog": {
        "title": "Blog",
        "breadcrumb": "Articles",
        "create": "Nouvel article"
    }
}
```

Les traductions sont chargées dynamiquement via `import.meta.glob`.

---

## Commandes CLI

### Gestion des extensions

```bash
php artisan extensions:scan                            # Scanner les manifests
php artisan extensions:sync                            # Synchroniser les permissions
php artisan extensions:list                            # Lister les extensions
php artisan extensions:install {id}                    # Installer (migrations)
php artisan extensions:install {id} --team=ID          # Installer pour une équipe
php artisan extensions:enable {id}                     # Activer globalement
php artisan extensions:enable {id} --team=ID           # Activer pour une équipe
php artisan extensions:disable {id}                    # Désactiver
php artisan extensions:uninstall {id}                  # Supprimer (rollback + nettoyage)
```

### Génération de code

```bash
php artisan extensions:make {slug}                     # Scaffold un module complet
php artisan make:theme {slug}                          # Scaffold un thème
php artisan make:module:model {slug} {name}            # Modèle + migration
php artisan make:module:controller {slug} {name}       # Contrôleur
php artisan make:module:policy {slug} {name}           # Policy
php artisan make:module:page {slug} {name}             # Page React
php artisan make:module:request {slug} {name}          # Form Request
php artisan make:module:service {slug} {name}          # Service
php artisan make:module:migration {slug} {name}        # Migration
php artisan make:module:test {slug} {name}             # Test Pest
php artisan make:extension:resource                    # Interactif
php artisan extension:stubs:publish                    # Publier les stubs
```

---

## Règles de compatibilité

- `provides` et `permissions` ne peuvent que **grandir** entre les versions — jamais diminuer
- Le `CompatibilityChecker` bloque les mises à jour qui violent ce contrat
- `minimum_core_version` est vérifié à l'installation

## Règles de sécurité

- **Toujours** utiliser `$user->hasPermissionTo()` pour les autorisations — jamais de vérification de rôle en dur
- **Toujours** utiliser le trait `HasTeam` sur les modèles — ils doivent être scoping par team
- **Toujours** vérifier les autorisations côté backend (Policies) — les guards frontend sont uniquement visuels
- Le rôle `owner` est protégé — il ne peut pas être renommé, modifié ou supprimé via l'UI
- Les routes de module sont automatiquement protégées par `EnsureTeamMembership`
- Les pages de module utilisent les imports `@/` (résolus par Vite)
- Lancer `php artisan wayfinder:generate` après avoir ajouté/modifié des routes

## Licence

MIT — OneSubnet
