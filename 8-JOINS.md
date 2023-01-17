# JOINS IN QUERIES

## Joins

A Join is the union of one or multiple tables to different purposes, in this case to show them

```SQL
    -- will return the union of both tables
    select * from customer as c
        join address as a on c.customer_id = a.address_id;

    /* always use the specific join 'inner, outer ...' */
    select * from customer as c 
        inner join address as a on c.customer_id = a.address_id;
    
    /* you can use the 'using' keyword to make the match, but it's better the 'on' clouse */
    select * from customer 
        inner join address using(address_id);
    
    -- you can use more flags or composed statements
    select * from customer 
        inner join address using(address_id)
    where address.postal_code = 52137;

    -- requesting multiple tables
    select c.first_name, a.address, ct.* 
        from customer as c 
            inner join address a on c.customer_id=a.address_id
            inner join city ct on a.address_id = ct.city_id;

    -- joins are use to make an union between tables
        -- they're used mostly for unions like:
            -- 1 to many, and viceversa
            -- many to many, with a aditional table
    select * from film as f
        inner join film_actor as fa on f.film_id=fa.film_id
        inner join actor as a on fa.actor_id=a.actor_id;

    -- Examples
    SELECT c.first_name, c.last_name, a.address, ct.city
        FROM customer c
            INNER JOIN address a ON c.address_id = a.address_id
            INNER JOIN city ct ON a.city_id = ct.city_id
    WHERE a.district = 'California';

    select * 
        from address a1 
        inner join address a2 on a1.city_id=a2.city_id and a1.address_id != a2.address_id;

    -- Examples
    select f.film_id, f.title, count(*) num_copies
    from film as f 
        inner join inventory as i
        on f.film_id = i.film_id
    group by f.film_id, f.title;
```

## LEFT OUTER and RIGHT OUTER

```SQL

    -- data in invetory is loaded into film, regartless if it has null data
        -- so left and right outer join will show null columns
    select f.film_id, f.title, count(i.inventory_id) num_copies
    from film as f
        left outer join inventory as i
        on f.film_id = i.film_id
    group by f.film_id, f.title;

    -- left outer join . on || right outer join . on
    -- they're used to add aditional rows to a table with:
    -- left outer join indicate film table will have aditional rows from inventory table
    -- and you will have to filter them en 'on'

    -- this takes the number of inventory for movies with id 13, 14 and 15, show null values in case exists
    select f.film_id, f.title, i.inventory_id
        from film f
        left outer join inventory i
            on f.film_id = i.film_id
        where f.film_id between 13 and 15;

    -- it changes the table which will support the concatenation of additional tables (inventory to film)
    SELECT f.film_id, f.title, i.inventory_id
        FROM inventory i
            RIGHT OUTER JOIN film f
            ON f.film_id = i.film_id
    WHERE f.film_id BETWEEN 13 AND 15;

    -- LOAD MULTIPLE TABLES WITH OUTER JOINS

    -- the results include all rentals of all films in inventory, but the film Alice Fantasia has
    -- null values for the columns from both outer-joined tables.
    select f.film_id, f.title, i.inventory_id, r.rental_date
        from film as f
            left outer join inventory as i
            on f.film_id = i.film_id
            left outer join rental as r
            on i.inventory_id = r.inventory_id
    WHERE f.film_id BETWEEN 13 AND 15;

    -- EXAMPLES

    select c.customer_id, concat(c.first_name," ",c.last_name) name, count(p.payment_id) pays, sum(p.amount) total_paid
    from customer as c
        left outer join (select payment_id, customer_id, amount from payment) as p
            on c.customer_id = p.customer_id
        group by c.customer_id
        order by 1;
    
    select c.customer_id, concat(c.first_name," ",c.last_name) name, count(p.payment_id) pays, sum(p.amount) total_paid
    from (select payment_id, customer_id, amount from payment) as p
        right outer join customer as c
            on p.customer_id = c.customer_id
    group by p.customer_id
    order by 1;
```

## CROSS JOIN

```SQL
    -- will make a mix between category and language
    select c.name caterogy_name, l.name language_name
    from category c
        cross join language l;

    -- returns nums from 1 to 100 using a cross join and simple math operation
    select ones.num + tens.num
    from (
            select 1 num union all
            select 2 num union all
            select 3 num union all
            select 4 num union all
            select 5 num union all
            select 6 num union all
            select 7 num union all
            select 8 num union all
            select 9 num union all
            select 10 num ) ones
        cross join
        (select 0 num union all
            select 10 num union all
            select 20 num union all
            select 30 num union all
            select 40 num union all
            select 50 num union all
            select 60 num union all
            select 70 num union all
            select 80 num union all
            select 90 num ) tens 
    order by 1;

    -- superb example of cross join, to count the number of rentals in a day, making a report of rentals
    SELECT days.dt, COUNT(r.rental_id) num_rentals
    FROM rental r
        RIGHT OUTER JOIN
            (SELECT DATE_ADD('2005-01-01',
                INTERVAL (ones.num + tens.num + hundreds.num) DAY) dt
            FROM
                (SELECT 0 num UNION ALL
                SELECT 1 num UNION ALL
                SELECT 2 num UNION ALL
                SELECT 3 num UNION ALL
                SELECT 4 num UNION ALL
                SELECT 5 num UNION ALL
                SELECT 6 num UNION ALL
                SELECT 7 num UNION ALL
                SELECT 8 num UNION ALL
                SELECT 9 num) ones
                CROSS JOIN
                (SELECT 0 num UNION ALL
                SELECT 10 num UNION ALL
                SELECT 20 num UNION ALL
                SELECT 30 num UNION ALL
                SELECT 40 num UNION ALL
                SELECT 50 num UNION ALL
                SELECT 60 num UNION ALL
                SELECT 70 num UNION ALL
                SELECT 80 num UNION ALL
                SELECT 90 num) tens
                CROSS JOIN
                (SELECT 0 num UNION ALL
                SELECT 100 num UNION ALL
                SELECT 200 num UNION ALL
                SELECT 300 num) hundreds
                WHERE DATE_ADD('2005-01-01',
                    INTERVAL (ones.num + tens.num + hundreds.num) DAY)
                    < '2006-01-01'
                ) days
                    ON days.dt = date(r.rental_date)
    GROUP BY days.dt
    ORDER BY 1;
```

## NATURAL JOINS

> Allows to skip writing the on clause after a join type

```SQL
    -- however the sakila database and in both tables exists the column 'last_update' and the server
    -- will select equals columns
    select c.first_name, c.last_name, date(r.rental_date) 
    from customer c 
        natural join rental r;
    
    -- show the same example avoiding 'last_update' column
    select c.first_name, c.last_name, date(r.rental_date)
    from ( select customer_id, first_name, last_name from customer ) as c
        natural join rental r limit 0,20000;

```
