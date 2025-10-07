 ---
hidden: true
---
# Magento 2 CLI

Magento 2 provides a powerful command-line interface (CLI) tool to manage and configure your store efficiently. Below is a categorized overview of all major `php bin/magento` commands, with explanations where needed.

---

## Setup & Installation

### Install & Upgrade

| Command | Description |
|--------|-------------|
| `setup:install` | Installs Magento with DB and admin configuration. |
| `setup:upgrade` | Applies DB schema/data changes from modules. |
| `setup:di:compile` | Compiles dependency injection code. |
| `setup:static-content:deploy` | Deploys static view files for frontend. |
| `setup:static-content:deploy -f` | Forces deployment even in developer mode. |
| `setup:uninstall` | Uninstalls Magento and removes configuration. |

### Config Status & Maintenance

| Command | Description |
|--------|-------------|
| `setup:config:set` | Sets configuration values like DB or admin URL. |
| `setup:db:status` | Shows database upgrade status. |
| `setup:db:status --show-config` | Shows current DB connection settings. |
| `maintenance:enable` | Enables maintenance mode. |
| `maintenance:disable` | Disables maintenance mode. |
| `maintenance:status` | Shows maintenance mode status. |

---

## Cache Management

| Command | Description |
|--------|-------------|
| `cache:clean` | Clears enabled cache types. |
| `cache:flush` | Clears all cache backends (e.g., Redis). |
| `cache:enable` | Enables specified cache types. |
| `cache:disable` | Disables specified cache types. |
| `cache:status` | Displays cache types and their status. |

---

## Index Management

| Command | Description |
|--------|-------------|
| `indexer:reindex` | Rebuilds indexers (e.g. product/category data). |
| `indexer:status` | Shows indexer status. |
| `indexer:reset` | Resets indexers that failed. |
| `indexer:info` | Lists all indexers. |
| `indexer:show-mode` | Displays indexer mode (schedule/realtime). |
| `indexer:set-mode {schedule|realtime} [indexer]` | Sets mode for a specific indexer. |

---

## Admin User Management

| Command | Description |
|--------|-------------|
| `admin:user:create` | Creates a new admin user. |
| `admin:user:unlock [username]` | Unlocks a locked admin account. |
| `admin:user:delete [username]` | Deletes an admin user. |

---

## Security

| Command | Description |
|--------|-------------|
| `security:passwords:upgrade` | Upgrades customer password hash format. |
| `security:hash:upgrade` | Upgrades internal hash algorithms for better security. |

---

## Module Management

| Command | Description |
|--------|-------------|
| `module:status` | Lists all modules and their status. |
| `module:enable [Module_Name]` | Enables a module. |
| `module:disable [Module_Name]` | Disables a module. |
| `module:uninstall [Module_Name]` | Uninstalls a module and optionally removes its data. |

---

## Cron & Job Scheduling

| Command | Description |
|--------|-------------|
| `cron:run` | Executes scheduled cron jobs immediately. |
| `cron:install` | Installs Magento cron jobs into system crontab. |

---

## Cleaning & Logs

| Command | Description |
|--------|-------------|
| `setup:upgrade --keep-generated` | Keeps generated code during upgrade (for production). |
| `dev:log:clean` | Clears system and exception logs. |

---

## Developer Tools

### Mode & Profiler

| Command | Description |
|--------|-------------|
| `deploy:mode:show` | Shows current deployment mode. |
| `deploy:mode:set {developer|production|default}` | Changes deployment mode. |
| `--skip-compilation` | Skips DI compilation during mode switch. |
| `dev:profiler:enable` | Enables performance profiler. |
| `dev:profiler:disable` | Disables the profiler. |

### Template & JS

| Command | Description |
|--------|-------------|
| `dev:template-hints:enable` | Displays template hints in frontend. |
| `dev:template-hints:disable` | Hides template hints. |
| `dev:source-theme:deploy` | Deploys source files (e.g. LESS) for custom themes. |

---

## Store Configuration

| Command | Description |
|--------|-------------|
| `config:set [path] [value]` | Sets a configuration value. |
| `config:show [path]` | Shows a configuration value. |
| `config:sensitive:set` | Marks config value as sensitive (e.g., passwords). |
| `config:sensitive:remove` | Removes sensitive flag from a config. |

---

## EAV (Entity-Attribute-Value)

| Command | Description |
|--------|-------------|
| `eav:attribute:remove` | Removes a custom attribute from the EAV model. |

---

## Inventory Management

| Command | Description |
|--------|-------------|
| `inventory:reservation:list-inconsistencies` | Lists mismatches in stock reservations. |
| `inventory:reservation:create-compensations` | Fixes reservation issues by creating compensation records. |

---

## Testing Tools

| Command | Description |
|--------|-------------|
| `dev:tests:run [unit|integration|functional]` | Runs specified test suite. |

---

## Search & Elasticsearch

| Command | Description |
|--------|-------------|
| `indexer:reindex catalogsearch_fulltext` | Reindexes product search data. |
| `config:set catalog/search/engine elasticsearch7` | Sets Elasticsearch as the search engine. |

---

## Backup & Restore

| Command | Description |
|--------|-------------|
| `setup:backup --code --media --db` | Creates backup of codebase, media, and DB. |
| `setup:rollback --code-file=... --db-file=...` | Restores from specified backup files. |

---

## Help

| Command | Description |
|--------|-------------|
| `list` | Lists all Magento CLI commands. |
| `help [command]` | Shows usage information for a command. |

---

> **Tip**: After module changes or config updates, always run `cache:clean`, `cache:flush`, and `setup:di:compile` to apply changes properly.
