# METADATA

Metadata is a collection of information that the RDBMS (server) collects from your databases, and can help you to management or administration of the database

```SQL
    -- returns all tables from different databases
        -- filter for 'sakila' database
    select * from information_schema.tables 
        where table_schema = 'sakila' and table_type != 'VIEW'
    order by table_type;
    
    -- returns all views created in different databases
        -- filter for 'sakila' database
    select * from information_schema.views
        where table_schema = 'sakila'
    order by 1;
    
    -- returns all the columns in differents tables from different databases
        -- filter for 'sakila' database
    select * from information_schema.columns
        where table_schema = 'sakila' AND table_name = 'film'
    order by 1;

    -- return information about indexes in columns from different tables from different databases
        -- filter for 'sakila' database
    select * from information_schema.statistics
        where table_schema = 'sakila' AND table_name = 'rental'
    order by 1;

    -- returns information about constraints in a database
        -- filter for 'sakila' database
    select * from information_schema.table_constraints
        where table_schema = 'sakila'
    order by 1;

    -- returns more information about constraints
    select * from information_schema.referential_constraints;

    -- returns permissions from your actual user
    select * from information_schema.user_privileges;

    -- returns character sets available in your database
    select * from information_schema.character_sets;

    --returns info about partitions in all databases
        -- filter for 'sakila' database
    select * from information_schema.partitions
    where table_schema = 'sakila';


    -- This will show all active & sleeping queries in that order then by how long
        --(the connected users)
    select * from information_schema.processlist order by info desc, time desc;

    select * from information_schema.routines where routine_definition like '%word%';

```
