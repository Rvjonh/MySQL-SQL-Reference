# UTILITY COMMANDS

List of commands usefull to work in database and SQL

```SQL
    -- shows all databases in the system or server
    show databases;

    -- creates a new database
    create database practice_sql;

    -- select the database to use and execute queries, ej. sakila
    use sakila;

    -- show all tables from a database
    show tables;

    -- gets all users from the server, if you have a root user you can watch them all
    select * from mysql.users;
    
    -- will show the columns and a simple struture of you table
    desc payment;
    --or (they're the same)
    describe payment;

    -- will show the command to create the table ...
    show create table mytable;
    
    -- returns the time in the system or server
    select @@global.time_zone, @@session.time_zone;

    -- return who controls the time
    select @@time_zone;

    -- show warnings from all sql queries or commands;
    show warnings;

    -- show [global | session] status; shows the server status
    SHOW VARIABLES;

    -- shows information about the query itself, to know its process and optimize it
    explain select * from user;
```

## Notes

* You can set in the prompt dialog, so 'CMD' or 'Terminal' your curren user and see it after any command line
