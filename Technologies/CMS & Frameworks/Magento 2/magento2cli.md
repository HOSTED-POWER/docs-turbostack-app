---
hidden: true
---
# Magento 2 CLI

Magento 2 provides a powerful command-line interface (CLI) tool to manage and configure your store efficiently. Below is a categorized overview of all major `php bin/magento` commands.

---

## Setup & Installation

### Install & Upgrade
```bash
php bin/magento setup:install
php bin/magento setup:upgrade
php bin/magento setup:di:compile
php bin/magento setup:static-content:deploy
php bin/magento setup:static-content:deploy -f
php bin/magento setup:uninstall
```

### Config Status & Maintenance
```bash
php bin/magento setup:config:set
php bin/magento setup:db:status
php bin/magento setup:db:status --show-config
php bin/magento maintenance:enable
php bin/magento maintenance:disable
php bin/magento maintenance:status
```

---

## Cache Management

```bash
php bin/magento cache:clean
php bin/magento cache:flush
php bin/magento cache:enable
php bin/magento cache:disable
php bin/magento cache:status
```

---

## Index Management

```bash
php bin/magento indexer:reindex
php bin/magento indexer:status
php bin/magento indexer:reset
php bin/magento indexer:info
php bin/magento indexer:show-mode
php bin/magento indexer:set-mode {schedule|realtime} [indexer]
```

---

## Admin User Management

```bash
php bin/magento admin:user:create
php bin/magento admin:user:unlock [username]
php bin/magento admin:user:delete [username]
```

---

## Security

```bash
php bin/magento security:passwords:upgrade
php bin/magento security:hash:upgrade
```

---

## Module Management

```bash
php bin/magento module:status
php bin/magento module:enable [Module_Name]
php bin/magento module:disable [Module_Name]
php bin/magento module:uninstall [Module_Name]
```

---

## Cron & Job Scheduling

```bash
php bin/magento cron:run
php bin/magento cron:install
```

---

## Cleaning & Logs

```bash
php bin/magento setup:upgrade --keep-generated
php bin/magento dev:log:clean
```

---

## Developer Tools

### Mode & Profiler
```bash
php bin/magento deploy:mode:show
php bin/magento deploy:mode:set {developer|production|default} [-s|--skip-compilation]
php bin/magento dev:profiler:enable
php bin/magento dev:profiler:disable
```

### Template & JS
```bash
php bin/magento dev:template-hints:enable
php bin/magento dev:template-hints:disable
php bin/magento dev:source-theme:deploy
```

---

## Store Configuration

```bash
php bin/magento config:set [path] [value]
php bin/magento config:show [path]
php bin/magento config:sensitive:set
php bin/magento config:sensitive:remove
```

---

## EAV (Entity-Attribute-Value)

```bash
php bin/magento eav:attribute:remove
```

---

## Inventory Management

```bash
php bin/magento inventory:reservation:list-inconsistencies
php bin/magento inventory:reservation:create-compensations
```

---

## Testing Tools

```bash
php bin/magento dev:tests:run [unit|integration|functional]
```

---

## Search & Elasticsearch

```bash
php bin/magento indexer:reindex catalogsearch_fulltext
php bin/magento config:set catalog/search/engine elasticsearch7
```

---

## Backup & Restore

```bash
php bin/magento setup:backup --code --media --db
php bin/magento setup:rollback --code-file=... --db-file=...
```

---

## Help

```bash
php bin/magento list
php bin/magento help [command]
```

---

> **Tip**: Always clear cache (`cache:clean` and `cache:flush`) and recompile (`setup:di:compile`) after enabling/disabling modules or making config changes.