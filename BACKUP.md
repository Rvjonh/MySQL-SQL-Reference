# BACKUP OF A DATABASE

Here the esensials commads for make a backup of a database.
The command to make backups is 'mysqldump' ...

`IN THE TERMINAL OR SHELL`

## BACKUP

```CMD
    # if you're in desktop/ the file will be there after the backup 
    mysqldump -u 'user' -p 'database_name' > 'database_backup.sql'
```

## IMPORT DATABASE

```CMD
    # Database should be created before ...
    # there are many others options for databases, e.x. for triggers, events, compressing, replication ...

    mysql -u 'user' -p 'database_name' < 'uri'[file of backup]
```

---

## NOTE: it's posible load data from file with

* LOAD DATA LOCAL INFILE
* it has multiple styles ... for txt, csv

---
