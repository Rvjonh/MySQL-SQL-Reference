# DELETION in tables

Example of a deletion in tables

```SQL
    --DELETE FROM table_name WHERE condition;

    -- deletes the first producto (generally), or product with id 1
    DELETE FROM products WHERE product_id=1;
```

## DELETE ALL RECORDS OR ROWS

`WARNING`
**DELETE FROM table_name;** will delete all records from the table, and most of the time is an error
prevent this problems with an IDE like WORKBENCH

In case delete all rows is what you are trying to use:

```SQL
    -- will re-create the table, and all rows will be gone (deleted)
    TRUNCATE products;
```
