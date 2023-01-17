# INDEXES

An index is a referencial point in the database, for optimization queries, when a column is used very often is recommendable to apply an index, so the database could make queries faster in that table through the index in the column

```SQL
    -- create index in custoemr table
    create index idx_email on customer (email);
    show index from customer;-- check indexes
    desc customer; -- describe the table

    -- delete index in table
        --also available 'drop index idx_email;'
    alter table customer drop index idx_email;

    -- adding unique index to table ...
        -- an unique index means that column will hace unique records, so no repeted like in a set
    alter table customer add unique idx_email (email);

    -- add double index
        -- the column on the left will be optimized individualy
        -- if a query is done with just 'amount' will not use the index, should required another index for the column
    alter table payment add index idx_payment_amount (payment_date, amount);

    -- deletes index by index_name
    alter table payment drop index idx_payment_amount;

    -- another option to create an index
    create index idx_payment_amount on payment (payment_date, amount);

```

## HOW TO USE INDEXES

```SQL
    -- the statement 'explain' can show you if you are using an index in the query correctly
    explain 
        select customer_id, first_name, last_name from customer
        where first_name like '%S' and last_name like '%P' ;
```

## CONTRAINS

> Allows to control what happens with a row is DELETE or UPDATED, and be an index or key to quick access between different join tables

```SQL
    -- examples:
    create table account (
        account_id int unsigned not null auto_increment,
        account_number varchar(20),
        address_id int unsigned not null,
        primary key (account_id), -- your foreign key                      #table   # its primary key
        constraint fk_account_address foreign key (address_id) references address (address_id)
    )ENGINE=InnoDB DEFAULT CHARSET=utf8;
    --recommended use engien=InnoDB ...

    SELECT c.first_name, c.last_name, c.address_id, a.address
    FROM customer c
        INNER JOIN address a
        ON c.address_id = a.address_id
    WHERE a.address_id = 123;

    -- add or alter  an constraint
    alter table rental 
        add constraint fk_rental_customer foreign key (customer_id)
        references customer (customer_id) on delete restrict on update cascade;

    show index from payment; -- show the new index

```
