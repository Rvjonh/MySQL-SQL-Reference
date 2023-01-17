# SQL - MySQL Documentry

Here are some of the aspect to consider of SQL in MySQL that I have used or learn

## Database

All the scripts or commands have been used in MySQL8.+.+, if any command doesn't work look for other document.

## Purpose

Here I will list some of my personal ideas of SQL and MySQL

## List of content or statements

* [DATA_TYPES](/0-DATA_TYPES.md)
* [UTILITY](/0-UTILITY.md)
* [CREATIONS](/1-CREATIONS.md)
* [DELETE](/1-DELETE.md)
* [INSERTS](/1-INSERTS.md)
* [SELECT](/1-SELECT.md)
* [UPDATE](/1-UPDATE.md)
* [STRINGS](/2-STRINGS.md)
* [DATE](/3-DATE.md)
* [MATH](/4-MATH.md)
* [UNION](/5-UNION.md)
* [AGGREGATE_FUNCTIONS](/6-AGGREGATE_FUNCTIONS.md)
* [SUBQUERIES](/7-SUBQUERIES.md)
* [JOINS](/8-JOINS.md)
* [CONDITIONALS](/9-CONDITIONALS.md)
* [TRANSACTIONS](/10-TRANSACTIONS.md)
* [INDEXES](/11-INDEXES.md)
* [VIEWS](/12-VIEWS.md)
* [METADATA](/13-METADATA.md)
* [FUNCTIONS](/14-FUNCTIONS.md)
* [BACKUP](/BACKUP.md)
* [MANAGEMENT](/MANAGEMENT.md)

## Data

All the exercises can be done with the sakila database available in MySQL website or you can use the file sakila_db.sql to load the tables and records from this repository.

inside your MySQL user:

```SQL
    create database sakila;
    mysql -u your_user -p sakila < sakila_db.sql
```

After this you have a new database and available to make SQL queries. I recomment you use a IDE like WorkBench from MySQL.

## Always use before any command

```SQL
    -- will select the sakila database
    use sakila;
```
