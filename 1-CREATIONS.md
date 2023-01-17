# CREATIONS OF BASIC OPTIONS

here are aspects which can be created from tables to triggers and more...

## TABLES

**for more different column data type check**: [DATA TYPES](/0-DATA_TYPES.md)

```SQL
    --- create a database
    create database mydb; -- don't forget run (use mydb;) to select it

    -- create a table in mysql
    create table mytable(
        id int unsigned not null auto_increment, -- an identificator, that comes from 1 to n (unsigned allow only positive numbers), auto_increment will control the id scaling itself
        username varchar(25) not null,
        email varchar(50) not null, comment 'this is a comment inside the table'
        nickname varchar(25) default 'new_user', -- can add default values
        genre enum("m", "f", "x"), -- enum allow only the list items
        primary key(id)
    )engine=InnoDB; -- sets the engine for the database, the better engine=InnoDB;

```

## TEMPORARY TABLE

Tables which will be deleted after session ends

```SQL
    -- create temporary table
    create temporary table student(
        student_id int unsigned not null auto_increment primary key,
        full_name varchar(64)
    );

    insert into student(full_name) values("jonh"),("carla"),("pepita"),("franchesca");
    select * from student;
    -- table and rows will be gone after you finish your session
```

## TRIGGERS

```SQL
    -- can be executed or configurated 'before' and 'after' with (delete, update, insert) events ...
        --example
    create table nums( numero int );

    -- to use the trigger.
    set @sum=0;
    -- for each row will execute the command for all rows or records
        -- before any insert on nums, the trigger will sum 1 a var
    create trigger sum_nums before insert on nums
    for each row set @sum = @sum + new.numero; 

    insert into nums values (1),(2),(3);    -- three insertions
    select * from nums; -- check them out
    select @sum;    -- and the counter works

```

```SQL
    -- shows the triggers
    show triggers;

    -- deletes the trigger by its name
    drop trigger sum_nums;

```

```SQL
    -- EXAMPLE
    -- after added a new record in 'mytable' its 'id' will be added to the nums tables too, by the after trigger ...
    create trigger add_id_to_nums 
        after insert on mytable
        for each row
    begin
        -- the keyword 'OLD' and 'NEW' allows control the data in
            -- the record in the database
            -- and the new record repectibly
        insert into nums (numero) values (NEW.id);
    end;
    
    -- insert in the table
    insert into mytable (username, email) values ("pedro","pedro@gmail.com");
```

## EVENTS

Really usefull to execute taks like clean after a time interval. Events are like triggers, but they are not called by a user's program. Rather, they are scheduled. As such, they succeed or fail silently.

```CMD
    An event can be executed by schedule in different time_stamps, ej:
    every 1 minute
    every 2 week
    every 1 month
    every 90 day
```

```SQL
    -- shows the events in the server
    SHOW VARIABLES WHERE variable_name='event_scheduler';
    -- show events from a database
    SHOW EVENTS FROM mydb;

    -- activate events
    SET GLOBAL event_scheduler = ON; -- TO TURN IT ON
    

    -- event will add 1 number to table nums
        -- an event which is executed every 1
    drop event if exists add_every_minute;
    create event add_every_minute
        on schedule every 1 minute starts '2023-01-12 00:00:00'
        on COMPLETION PRESERVE 
    do begin 
        insert into nums (numero) values (last_insert_id());
    end;

    --EXAMPLE
    CREATE EVENT insertion_event
    ON SCHEDULE AT CURRENT_TIMESTAMP + INTERVAL 1 MINUTE
    DO INSERT INTO test VALUES ('Evento 1', NOW());

    -- STRUCTURE

    CREATE [DEFINER = { user | CURRENT_USER }]
    EVENT [IF NOT EXISTS] event_name
    ON SCHEDULE schedule
    [ON COMPLETION [NOT] PRESERVE]
    [ENABLE | DISABLE | DISABLE ON SLAVE]
    [COMMENT 'string']
    DO event_body;

schedule:
    AT timestamp [+ INTERVAL interval] ...
    |EVERY interval
    [STARTS timestamp [+ INTERVAL interval] ...]
    [ENDS timestamp [+ INTERVAL interval] ...]

    interval:
        quantity {YEAR | QUARTER | MONTH | DAY | HOUR | MINUTE |
                WEEK | SECOND | YEAR_MONTH | DAY_HOUR | DAY_MINUTE |
                DAY_SECOND | HOUR_MINUTE | HOUR_SECOND | MINUTE_SECOND}

```
