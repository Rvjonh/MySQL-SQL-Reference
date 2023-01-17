# SELECT

Notes about select

```SQL
-- can select all the columns from a table with ' * ' wildcard, (pd: not recommended for production queries) 
select * from customer;

-- you can set and alias with 'as' keyword
-- use the dot notation to select the columns to show
-- 'order by' will sort the columns in 'ASC' ascending or 'DESC' descending way
select a.actor_id, a.last_name, a.first_name 
    from actor as a 
order by a.last_name, a.first_name;

-- will filter the results and show unique cases from that column, apply a set data structure function
select distinct address_id, a.* from address a;

-- you can filter the elements with 'where'
-- there (and, or) operators
select a.actor_id, a.last_name, a.first_name 
    from actor as a 
WHERE a.last_name = 'WILLIAMS' or a.last_name = 'DAVIS';

-- the 'in' will look for inside a list of elements "which can be a subquery"
select a.actor_id, a.last_name, a.first_name 
    from actor as a
WHERE a.last_name IN ('WILLIAMS','DAVIS');

-- operator '<>' is equal to '!=', which means 'not' or different of
select * from payment p where p.customer_id <> 5;

-- can compare dates
select distinct r.customer_id 
    from rental as r 
WHERE date(r.rental_date) = '2005-07-05';

-- can set a numerical range with the keyword 'between'
select p.payment_id, p.customer_id, p.amount, date(payment_date) 
    from payment as p 
where p.payment_id between 101 and 120;

-- like is used to filter in string character columns,
    -- % used to identify additional chars
    -- _ used to identify one char
    -- for more advanced string queries, better use 'regexp'
    -- for more advanved text queries, better use 'MATCH'
select * from customer c where c.last_name like '_A%W%';

-- REGEXP pattern in SQL and in 'where' clauses
select last_name, first_name 
    from customer
where last_name regexp '^[QY]';

-- another REGEXP example
select * from customer c where c.last_name regexp '^.{1}[a]{1}.*[w]{1}.*';

-- create composed queries
SELECT c.email, r.return_date
    FROM customer as c
    INNER JOIN rental as r
        ON c.customer_id = r.customer_id
WHERE date(r.rental_date) = '2005-06-14'
ORDER BY r.return_date desc, c.email ;


```
