# SUBQUERIES

A subqueries is a query inserted in others queries.

Subqueries can be extracted or treated in 3 cases:

* a query with a row and a column, like:

```SQL
    column> |name|
    record> |jonh|
```

* a query with several rows and a column, like:

```SQL
    column> |name|
    record> |jonh|
    record> |fran|
    record> |ivan|
```

* a query with several rows and several columns, like:

```SQL
    columns> |name| age | genre |
    record>  |jonh| 21  |  "m"  |
    record>  |sara| 20  |  "f"  |
    record>  |ivan| 33  |  "x"  |
```

```SQL
    --Using another query inside a query to use its retrived data
        -- selects the customer with the biggest id, normally the last one
    select * from customer 
    where customer_id = (select MAX(customer_id) from customer);

    -- select all the city which are not in India
    select * from city 
    where country_id != (select country_id from country where country = 'India');

    -- to filter with multi row single column use operator 'in'
    select * from country 
    where country in ('Canada', 'Mexico');

    -- select all of the cities from 'Canada' and 'Mexico' ...
    select * from city 
    where country_id in (select country_id from country where country in ('Canada', 'Mexico'));

    -- select all of the cities which are not from 'Canada' and 'Mexico' ...
    select * from city 
    where country_id not in (select country_id from country where country in ('Canada', 'Mexico'));

    -- Dealing with multiple columns and rows
    -- you can suctract multiple colums from a subquery
    SELECT actor_id, film_id
    FROM film_actor
    WHERE (actor_id, film_id) IN (SELECT a.actor_id, f.film_id
                                    FROM actor a
                                    CROSS JOIN film f
                                    WHERE a.last_name = 'MONROE'
                                    AND f.rating = 'PG');

    -- select clients which has recieved a payment with 0 cost, or for free ...
    select * from customer
    where customer_id <> all (select customer_id from payment where amount = 0);
```

## Use the Oeprator ANY

```SQL
    -- Although most people prefer to use 'in', using 'any' is equivalent to using the 'in' operator.
        -- The subquery returns the total film rental fees for all customers in Bolivia, Paraguay,
        -- and Chile, and the containing query returns all customers who outspent at least one
        -- of these three countries
    SELECT customer_id, sum(amount)
    FROM payment
    GROUP BY customer_id
        HAVING sum(amount) > ANY (SELECT sum(p.amount)
                                    FROM payment p
                                    INNER JOIN customer c
                                        ON p.customer_id = c.customer_id
                                    INNER JOIN address a
                                        ON c.address_id = a.address_id
                                    INNER JOIN city ct
                                        ON a.city_id = ct.city_id
                                    INNER JOIN country co
                                        ON ct.country_id = co.country_id
                                    WHERE co.country IN ('Bolivia','Paraguay','Chile')
                                    GROUP BY co.country);

```

## Correlated Subqueries

```SQL
    -- You can use your primary query with a alias and insert it in the subquery
    -- all the actors which has gotten 20 rentals
    SELECT c.first_name, c.last_name
        FROM customer c
    WHERE 20 = (SELECT count(*) FROM rental r WHERE r.customer_id = c.customer_id);
```

## Uncorrelated subquery

> An uncorrelated subquery a query which has no interaction out of its scope, and works without external data

```SQL
    --exampe uncorrelated subquery
    select * from customer 
    where customer_id = (select MAX(customer_id) from customer);
```

## Operator 'EXISTS'

```SQL
    -- Using the exists operator, your subquery can return zero, one, or many rows, and
    -- the condition simply checks whether the subquery returned one or more rows.
    SELECT a.first_name, a.last_name
    FROM actor a
    WHERE EXISTS (SELECT 1 FROM film_actor fa
                    INNER JOIN film f ON f.film_id = fa.film_id
                    WHERE fa.actor_id = a.actor_id AND f.rating = 'R');
```

## Subqueries long work using an alias

```SQL
    -- you can make an inner join with a subquery, given it an alias
    select c.first_name, c.last_name, pymt.num_rentals, pymt.tot_payments
    from customer c
    inner join ( select customer_id, count(*) num_rentals, sum(amount) tot_payments 
                    from payment
                    group by customer_id
                ) pymt
    on pymt.customer_id = c.customer_id;
```
