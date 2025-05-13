---
hidden: true
---

# MySQL

## MySQL Import & Export

We always recommend to dump and import through the command line, if possible. This documentation will explain how to do this securely.

### MySQL Export
For most databases (only a couple of gigabytes big), you can dump the database with this command:
```bash
mysqldump --single-transaction --triggers --routines --events -y DBNAME > DBNAME.sql
```

For larger databases (tens of gigabytes or bigger), compress the database to conserve disk space and speed up transfer:

```bash
mysqldump --single-transaction --triggers --routines --events -y DBNAME | gzip -3 -v > DBNAME.gz
```

### MySQL Import
```
mysql DBNAME < DBNAME.sql
```
### Transferring the Database

Use `scp` to transfer the file:

```bash
scp FileName user@HostnameOrIP:Path/To/Folder
```

> **Hint:** To place the file in the user's home directory, remove everything after the colon.

### Importing the Database

```bash
mysql DBNAME < DBNAME.sql
```

Or, if compressed:

```bash
gunzip -c DBNAME.gz | mysql DBNAME
```

> **Hint:** Large databases can take time to import, especially on high-load servers.

---
## Extra Tips

### Nohup

Use `nohup` to ensure the dump or import continues if the connection is lost.

### Screen

Use `screen` to allow session sharing or recovery:

- Create a session:

  ```bash
  screen -S <session_name>
  ```

- Disconnect (keep running):

  ```
  Ctrl + A, then D
  ```

- Reattach or take over session:

  ```bash
  screen -dr <session_name>
  ```

- List sessions:

  ```bash
  screen -ls
  ```

### SSH Key

Avoid password prompts by setting up SSH key authentication.

---

## Fixes for Potential Errors

### MySQL ERROR 1227 (42000)

> *Access denied; you need (at least one of) the SUPER privilege(s) for this operation*

This is caused by `routines`, `views`, or `triggers` defined with a definer the user cannot access.

**Solution:** Strip the definer using `sed`:

```bash
mysqldump --routines --single-transaction --triggers --routines --events -y DBNAME | sed -e 's/DEFINER=[^*]*\*/\*/g' > DBNAME.sql
```

**Or with compression:**

```bash
mysqldump --single-transaction --triggers --routines --events -y DBNAME | sed -e 's/DEFINER=[^*]*\*/\*/g' | gzip -3 -v > DBNAME.gz
```

**If you already have the file:**

```bash
cat DBNAME.sql | sed -e 's/DEFINER=[^*]*\*/\*/g' | mysql DBNAME
```

**Compressed file:**

```bash
gunzip -c DBNAME.gz | sed -e 's/DEFINER=[^*]*\*/\*/g' | mysql DBNAME
```

---

### MySQL ERROR 1118 (42000)

> *Row size too large (> 8126). Changing some columns to TEXT or BLOB or using ROW_FORMAT=DYNAMIC or ROW_FORMAT=COMPRESSED may help.*

Caused by limits on row size when using `ROW_FORMAT=COMPACT`.

**Fix during import:**

```bash
cat DBNAME.sql | sed -e 's/ROW_FORMAT=COMPACT/ROW_FORMAT=DYNAMIC/g' | mysql DBNAME
```

**Compressed file:**

```bash
gunzip -c DBNAME.gz | sed -e 's/ROW_FORMAT=COMPACT/ROW_FORMAT=DYNAMIC/g' | mysql DBNAME
```