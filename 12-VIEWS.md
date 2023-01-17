# VIEWS

A view can be an interface to secret hide information for many or other pursoses, like abstract complex queries.

```SQL
    -- exaple
    CREATE VIEW customer_vw (customer_id, first_name, last_name, email )
    AS 
    SELECT customer_id, first_name, last_name, concat(substr(email,1,2), '*****', substr(email, -4)) email 
    FROM customer;
    
    show tables; -- a view can be see in tables
    select * from customer_vw;  -- and treat it like a table, indeed
    delete from customer_vw where customer_id = "1"; -- depending on the view you could interact with the records
    describe customer_vw;   -- check its structure

    -- using the view
    select first_name, count(first_name)
    from customer_vw
    group by first_name
        having count(first_name) > 1;

    -- can add indexes to the views
    show index from customer_vw;
```

```SQL
    -- another example
    create table azucar(
        azucar_id int unsigned auto_increment,
        name varchar(10) not null,
        primary key (azucar_id)
    );

    insert into azucar (azucar_id, name) values (null, "negra");
    select * from  azucar;

    create view azucar_vw (azucar_id, name) as select * from azucar;
    show tables;

    delete from azucar_vw where azucar_id="1";
```

## Examples

```SQL
    -- returns every film (repeated) with its actors
    create view film_ctgry_actor as 
    select f.title, c.name as category_name, a.first_name, a.last_name from actor as a
        inner join film_actor as fa on a.actor_id = fa.actor_id
        inner join film as f on f.film_id = fa.film_id
        inner join film_category as fc on f.film_id = fc.film_id
        inner join category as c on fc.category_id = c.category_id;

    show tables;
    select * from film_ctgry_actor WHERE last_name = 'FAWCETT';
```

```SQL
    -- returns a report of pays from every country
    create view report_pays_country
    as
    select cn.country, count(p.customer_id) payments_country, sum(p.amount) from customer as c
        inner join address as ad on c.address_id = ad.address_id
        inner join city as ct on ad.city_id = ct.city_id
        inner join country as cn on ct.country_id = cn.country_id 
        inner join payment as p on c.customer_id = p.customer_id
        group by cn.country;

    select * from report_pays_country;
```
