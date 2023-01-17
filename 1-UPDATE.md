# UPDATE

Demostration of update process in a table that comes from a database;

```SQL
    -- will update the username with the id '2' in mytable
    update mytable set username= "carlitox" where id=2;
    
    -- STRUCTURE
    INSERT [LOW_PRIORITY | DELAYED | HIGH_PRIORITY] [IGNORE]
        [INTO] tbl_name [PARTITION (partition_list)] [(col,...)]
        {VALUES | VALUE} ({expr | DEFAULT},...),(...),...
        [ ON DUPLICATE KEY UPDATE
            col=expr
            [, col=expr] ... ]
```
