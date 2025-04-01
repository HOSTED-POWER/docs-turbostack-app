---
order: 200
icon: columns
---

# MySQL

## MySQL Import & Export

For large databases we recommend exporting and importing trough the commandline

### MySQL Export
```
mysqldump --single-transaction --triggers --routines --events -y DBNAME > DBNAME.sql
```
OR with gzip (much smaller)
```
mysqldump --single-transaction --triggers --routines --events -y DBNAME | gzip -3 -v > DBNAME.gz
```

### MySQL Import
```
mysql DBNAME < DBNAME.sql
```

### Potential errors & resolution

* ERROR 1227 (42000) at line xxx: Access denied; you need (at least one of) the SUPER privilege(s) for this operation
* SQLSTATE[42000]: Syntax error or access violation: 1142 TRIGGER command denied to user 'xxx'@'%' for table 'xxx', query was: ...
* ERROR 1449 (HY000) at line xxx: The user specified as a definer ('xxx'@'%') does not exist, query was: ...
  
* ERROR 1227 (42000) at line xxx: Access denied; you need (at least one of) the SUPER or SET_USER_ID privilege(s) for this operation
This is most likely because your database has routines/views, make sure to strip the definer owner during export:
```
mysqldump --routines --single-transaction --triggers --routines --events -y DBNAME | sed -e 's/DEFINER=[^*]*\*/\*/g' > DBNAME.sql
```
OR
```
mysqldump --single-transaction --triggers --routines --events -y DBNAME | sed -e 's/DEFINER=[^*]*\*/\*/g' | gzip -3 -v > DBNAME.gz
```
Or if you have already a dump file, you can also remove it during import:
```
cat DBNAME.sql | sed -e 's/DEFINER=[^*]*\*/\*/g' | mysql DBNAME
```

* mysqldump: Error: 'Access denied; you need (at least one of) the PROCESS privilege(s) for this operation' when trying to dump tablespaces
If you see this error, make sure to use the "-y" argument as in the above example when dumping MySQL database (This is the no-tablespaces option), your dump will still contain everything needed.
* mysql: ERROR 1118 (42000) at line xxx: Row size too large (> 8126). Changing some columns to TEXT or BLOB or using ROW_FORMAT=DYNAMIC or ROW_FORMAT=COMPRESSED may help. In current row format, BLOB prefix of 768 bytes is stored inline.
If you see this error, probably your data has grown too large to still be imported/exported with the ROW_FORMAT=COMPACT. You can import most likely by converting to ROW_FORMAT=DYNAMIC with the following command
```
cat DBNAME.sql | sed -e 's/ROW_FORMAT=COMPACT/ROW_FORMAT=DYNAMIC/g' | mysql DBNAME
```

## MySQL Charset & collation fix

_Example script to convert a whole database to a certain charset and collation_ 

cat fixcollaction.bash
```
#!/bin/bash

DB="$1"
CHARSET="$2"
COLL="$3"

[ -n "$DB" ] || exit 1
[ -n "$CHARSET" ] || CHARSET="utf8mb4"
[ -n "$COLL" ] || COLL="utf8mb4_unicode_ci"

echo $DB
echo "ALTER DATABASE \`$DB\` CHARACTER SET $CHARSET COLLATE $COLL;" | mysql

echo "USE \`$DB\`; SHOW TABLES;" | mysql -s | (
    while read TABLE; do
        echo $DB.$TABLE
        echo "ALTER TABLE \`$TABLE\` CONVERT TO CHARACTER SET $CHARSET COLLATE $COLL;" | mysql $DB
    done
)
```
