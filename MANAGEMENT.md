# MANAGEMENT OF A DATABASE

Here are are posted some process that and administrator would do in databases, like create a new user with permisions.

```SQL
    -- creates a new user, just that.
    create user 'jonh'@"localhost" identified by 'jonh1015'; -- last string is the password

    -- gives 'select, update, insert' permisions to user 'jonh' in localhost ...
    grant select, update, insert on mydb.* to 'jonh'@'localhost';

    -- gives all permisions on all databases to the 'user' in localhost, like a root user
    grant all on *.* to 'user'@'localhost' with grant option;

    -- gets all users from the server
    select * from mysql.users;

    -- another way show grants from a user
    show grants for 'user'@'%'; -- fill user and '%' like the host

    -- drops so deletes the user
    drop user 'user'@'%';

    -- This will show all active & sleeping queries in that order then by how long
        --(the connected users)
    select * from information_schema.processlist order by info desc, time desc;

    select * from information_schema.routines where routine_definition like '%word%';
```

## PARTITIONS

### TYPES

* by range, for example for dates, like divide recods agains years, or number of records.
* by list, assign elements to gruops of values in a column to devide depending his values.
* by hash, you can define a number of the group like 4 and el table will spred the elements into these 4 hash.
* and can be composed, so can have hash and list.

## LOGGING

```SQL
    -- show information about logs in the server
    SELECT @@general_log;
    SELECT @@general_log_file;
    SELECT @@datadir;
```

## REPLICATION

Replication, storing same process or queries in another database server

```SQL
    -- show info related
    SHOW MASTER STATUS;
    SHOW SLAVE STATUS;
```

## OPTIMIZING MYSQL

### Basics

* default_storage_engine = InnoDB
* query_cache_type = 0
* innodb_file_per_table = 1
* innodb_flush_neighbors = 0

### Concurrency

* innodb_thread_concurrency = 0
* innodb_read_io_threads = 64
* innodb_write_io_threads = 64

### Hard drive utilization

* innodb_io_capacity = 2500
* innodb_io_capacity_max = 3000

### RAM utilization will set 10gb of ram

* innodb_buffer_pool_size = 10G
