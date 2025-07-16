---
hidden: true
---
# WP-CLI Commands

WP-CLI is the command-line interface for managing WordPress installations. Below is a categorized reference guide with the most useful commands and their descriptions.

---

## Core Management

| Command | Description |
|--------|-------------|
| `wp core download` | Downloads WordPress core files. |
| `wp core install` | Runs the WordPress installation. |
| `wp core update` | Updates WordPress core to the latest version. |
| `wp core version` | Displays the current WordPress version. |
| `wp core check-update` | Checks for available core updates. |

---

## Plugin Management

| Command | Description |
|--------|-------------|
| `wp plugin install <plugin>` | Installs a plugin by slug or URL. |
| `wp plugin activate <plugin>` | Activates a plugin. |
| `wp plugin deactivate <plugin>` | Deactivates a plugin. |
| `wp plugin delete <plugin>` | Deletes a plugin. |
| `wp plugin update <plugin>` | Updates a specific plugin. |
| `wp plugin list` | Lists installed plugins with status. |

---

## Theme Management

| Command | Description |
|--------|-------------|
| `wp theme install <theme>` | Installs a theme by slug or URL. |
| `wp theme activate <theme>` | Activates a theme. |
| `wp theme delete <theme>` | Deletes a theme. |
| `wp theme update <theme>` | Updates a specific theme. |
| `wp theme list` | Lists installed themes. |

---

## User Management

| Command | Description |
|--------|-------------|
| `wp user create <username> <email>` | Creates a new user. |
| `wp user delete <user>` | Deletes a user. |
| `wp user list` | Lists all users. |
| `wp user update <user>` | Updates user information. |
| `wp user get <user>` | Displays user data. |

---

## Database Management

| Command | Description |
|--------|-------------|
| `wp db export` | Exports the database to an SQL file. |
| `wp db import <file>` | Imports an SQL file into the database. |
| `wp db reset` | Drops all tables and reinitializes the DB. |
| `wp db check` | Checks database for errors. |
| `wp db optimize` | Optimizes the database. |

---

## Media Management

| Command | Description |
|--------|-------------|
| `wp media import <file(s)>` | Imports media files into the Media Library. |
| `wp media regenerate` | Regenerates image sizes for Media Library. |

---

## Post & Content Management

| Command | Description |
|--------|-------------|
| `wp post create` | Creates a new post. |
| `wp post delete <id>` | Deletes a post. |
| `wp post list` | Lists posts. |
| `wp post update <id> --post_title="New Title"` | Updates post fields. |

---

## Option & Config Management

| Command | Description |
|--------|-------------|
| `wp option get <name>` | Gets an option value. |
| `wp option update <name> <value>` | Updates an option value. |
| `wp option delete <name>` | Deletes an option. |
| `wp config create` | Creates a `wp-config.php` file. |
| `wp config set <key> <value>` | Sets a config value. |
| `wp config get <key>` | Gets a config value. |

---

## Cron Events

| Command | Description |
|--------|-------------|
| `wp cron event list` | Lists scheduled cron events. |
| `wp cron event run <hook>` | Executes a cron event immediately. |
| `wp cron event delete <hook>` | Deletes a cron event. |

---

## Cache & Transients

| Command | Description |
|--------|-------------|
| `wp cache flush` | Clears the object cache. |
| `wp transient get <name>` | Gets a transient value. |
| `wp transient set <name> <value> <expiration>` | Sets a transient. |
| `wp transient delete <name>` | Deletes a transient. |

---

## Search & Replace

| Command | Description |
|--------|-------------|
| `wp search-replace <old> <new>` | Replaces strings in DB. |
| `--dry-run` | Simulates the operation without changes. |

---

## Miscellaneous

| Command | Description |
|--------|-------------|
| `wp site url` | Shows the site URL. |
| `wp rewrite flush` | Flushes rewrite rules. |
| `wp eval <php>` | Runs PHP code in WP context. |
| `wp shell` | Opens an interactive PHP shell. |
| `wp cli update` | Updates WP-CLI itself. |

---

> **Tip**: You can run `wp help` or `wp help <command>` to see usage examples and options for any WP-CLI command.
