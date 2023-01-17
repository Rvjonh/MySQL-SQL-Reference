# INSERTIONS IN TABLES

Demostration of insertion in a table that comes from a database.

```SQL
    -- use sakila;

    insert into actor (first_name, last_name) -- write the columns to fill, will raise an exeption in case a column is null and cannot ne nullish
            values ("jonh", "travolta"); -- fill up with the values, in order and correct datatype

```

## UPDATE IF IT'S DUPLICATED THE RECORD

```SQL
    -- example
    insert into mytable (username, email) 
    values ("rvjonh", "jonhvelasco3@gmail.com") as newInput
    on duplicate key update -- ALLOWS to control the data inputs
        id = last_insert_id(id),
        username = 'rvjonh2', 
        email = newInput.email;
        
```
