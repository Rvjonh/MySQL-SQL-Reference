# AGGREGATE FUNCTIONS

An aggregate function in SQL performs a calculation on multiple values and returns a single value

```SQL
    -- can group columns with the flag 'group by [column]'
        -- now perform aggregate functions
    -- count items in the table
    select customer_id, count(*) from rental 
    group by customer_id 
    order by 2 desc;
        
    -- having restrict under a 'group by' statement
        -- you must only use 'having' with 'group by'    
    select customer_id, count(*) from rental 
    group by customer_id 
        having count(*) >= 40;
    
    -- grouped with the year
    SELECT extract(YEAR FROM rental_date) year,
            COUNT(*) how_many
    FROM rental
    GROUP BY extract(YEAR FROM rental_date);

    -- returns sum from all the customers
        -- sum() returns the sum of a multiple number list
    select customer_id, count(payment_id) as 'payments', sum(amount) as 'total_paid'
    from payment
    group by customer_id;

    -- returns the bigger number from a numeric list
    select MAX(customer_id) from customer;

```
