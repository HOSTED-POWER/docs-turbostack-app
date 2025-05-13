---
hidden: true
---

# PostgreSQL

## PostgreSQL Import & Export

We always recommend to dump and import through the command line, if possible. This documentation will explain how to do this securely.

---

### PostgreSQL Export

For most databases (only a couple of gigabytes big), you can dump the database with this command:

```bash
pg_dump -U DBUsername DatabaseName > DBNAME.sql
```
For larger databases (tens of gigabytes or bigger), compress the database to conserve disk space and speed up transfer:
```bash
pg_dump -U DBUsername DatabaseName | gzip -3 -v > test.gz
```

### Transferring the Database
Use `scp` to transfer the file:

```bash
scp FileName user@HostnameOrIP:Path/To/Folder
```

> **Hint:** To place the file in the user's home directory, remove everything after the colon.

### Importing the Database

```bash
psql -U DBUsername DatabaseName < dbname
```
Or, if compressed:
```bash
gunzip -c dbname.gz | psql -U DBUsername DatabaseName
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
