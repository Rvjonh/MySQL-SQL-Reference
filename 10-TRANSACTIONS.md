# TRANSACTIONS

> A transaction is a process which allows you control what is execute in the server
> being able of keep a savepoint which represents the pass state of the database
> allows to controls bad process or exections where a command can't be execute

## **Disclaimer or Warnning**

`YOU SHOULD EXECUTE THIS COMMAND TO PREVENT THE COMMIT IN TRANSACTION`
> SET autocommit=0;

-- and check if it's correctly off

> show session variables like '%autocommit%';

```SQL
    -- example
    start transaction;  -- init the transaction
    select * from customer;
    savepoint before_change_name; -- save or keep a point in the present of you data
    update customer set name = 'Jonh Salchichon' where id = 1; -- any change is executed
    select * from customer; -- checkint it out
    rollback to savepoint before_change_name; -- allows you to come back to the point in the pass now
    select * from customer; -- and now it's like in the beginning
    commit;
```

```SQL
    -- Generate a unit of work to transfer $50 from account 123 to account 789. You will
    -- need to insert two rows into the transaction table and update two rows in the
    -- ccount table. Use the following table definitions/data:
    start transaction;
    savepoint none_tables;

    create table account (
        account_id int unsigned not null auto_increment,
        avail_balance long not null,
        last_activity_date timestamp,
        primary key (account_id)
    );

    create table transaction (
        txn_id int unsigned not null auto_increment,
        txn_date timestamp,
        account_id int not null,
        txn_type_cd char(3),
        amount int,
        primary key (txn_id)
    );

    -- rollback to savepoint -- in case you want to delete the tables in the database
    savepoint none_data_inserted;

    -- accounts ...
    insert into account (account_id, avail_balance, last_activity_date)
    values (123, 500, '2019-07-10 20:53:27');
    insert into account (account_id, avail_balance, last_activity_date)
    values (789, 75, '2019-06-22 15:18:35');

    insert into transaction (txn_id, txn_date, account_id, txn_type_cd, amount) 
    values (1001, '2019-05-15', '123', 'C', 500);
    insert into transaction (txn_id, txn_date, account_id, txn_type_cd, amount) 
    values (1002, '2019-06-01', 789, 'C', 75);

    -- rollback to savepoint none_data_inserted; -- in case you want to have empty tables

    savepoint new_transactions;

    update account
        set avail_balance = avail_balance - 50, last_activity_date = now()
    where account_id = 123 and avail_balance >= 50;

    update account
        set avail_balance = avail_balance + 50, last_activity_date = now()
    where account_id = 789;

    insert into transaction (txn_id, txn_date, account_id, txn_type_cd, amount) 
        values (null, now(), 123, 'D', 50);
    insert into transaction (txn_id, txn_date, account_id, txn_type_cd, amount) 
        values (null, now(), 789, 'C', 50);

    rollback to savepoint new_transactions; -- in case you want to insert again the reported transactions

    commit;
```

```SQL
    -- example
    START TRANSACTION;  -- init the transaction

    --any changes or commands
    UPDATE product
        SET date_retired = CURRENT_TIMESTAMP()
    WHERE product_cd = 'XYZ';

    SAVEPOINT before_close_accounts; -- saves a point to come back in any error case, savepooint name_point;

    UPDATE account
        SET status = 'CLOSED', close_date = CURRENT_TIMESTAMP(),
            last_activity_date = CURRENT_TIMESTAMP()
    WHERE product_cd = 'XYZ';

    ROLLBACK TO SAVEPOINT before_close_accounts; -- returns to the point state before
    COMMIT; -- executes all the commands in the transaction
```
