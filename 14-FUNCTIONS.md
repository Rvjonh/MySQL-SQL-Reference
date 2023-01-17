# FUNCTIONS

A list of useful functions

## RANKING

```SQL
    -- TYPE OF RANKINGS
        -- row_number() ranks in order, doesn't care ties
        -- rank() ranks in order, and when ties counts the positions and doesn't show them
        -- dense_rank() ranks in order, and when ties keep counting the positions
    select r.customer_id, count(r.customer_id),
        row_number() over (order by count(*) desc) row_num_num,
        rank() over (order by count(*) desc) rank_num,
        dense_rank() over (order by count(*) desc) dense_rank_num
    from rental as r
    group by r.customer_id
    order by 2 desc;

    -- Ranking sorted by a partition and ordered by the monthname
    select r.customer_id, 
            monthname(r.rental_date) rental_month,
            count(*),
            rank() over (partition by monthname(r.rental_date) order by count(*) desc) rank_rank
    from rental as r
    group by r.customer_id, rental_month
    order by 2,3 desc;

    -- Example with subqueries
    SELECT customer_id, rental_month, num_rentals, rank_rnk ranking
    FROM( select r.customer_id customer_id, 
                monthname(r.rental_date) rental_month,
                count(*) num_rentals,
                rank() over (partition by monthname(r.rental_date) order by count(*) desc) rank_rnk
            from rental as r
            group by r.customer_id, rental_month
            order by 2,3 desc
        )cust_rankings
    WHERE rank_rnk <= 5
    ORDER BY rental_month, num_rentals desc, rank_rnk;
```

## REPORTING FUNCTIONS

```SQL

    -- through a partition in the data set
        -- it gets month name and the total sum of the amount
    select monthname(p.payment_date) payment_month, p.amount,
            sum(amount) over (partition by monthname(p.payment_date)) monthly_total,
            sum(amount) over() grand_total
    from payment as p
    where p.amount > 10
    order by 1;

    -- using case conditions
    select monthname(p.payment_date) month_name,
            sum(p.amount) month_total,
            case sum(p.amount)
                when max(sum(p.amount)) over () then 'highest'
                when min(sum(p.amount)) over () then 'lowest'
                else 'middle'
            end Descriptor
    from payment as p
    group by monthname(p.payment_date);
```

## LAG AND LEAD

> FUNCTIONS TO RETRIVE PAST AND FUTURE COLUMNS VALUE

* LAG FOR PAST COLUMNS VALUE
* LEAD FOR FUTURE COLUMNS VALUE

```SQL
    -- return the last and follow row
    select yearweek(p.payment_date) payment_week,
            sum(p.amount) week_total,
            lag(sum(p.amount), 1) over (order by yearweek(p.payment_date)) prev_week_total,
            lead(sum(p.amount), 1) over (order by yearweek(p.payment_date)) next_week_total
    from payment as p
    group by yearweek(p.payment_date)
    order by 1;
        

    --Concating stings
    select f.title, 
            group_concat(concat(a.first_name, " ", a.last_name) separator ', ') actors
    from actor as a
    inner join film_actor as fm on a.actor_id = fm.actor_id
    inner join film as f on fm.film_id = f.film_id
    group by f.title;

    select *,
            lag(tot_sales, 1) over(order by month_no) last_month
    from Sales_Fact as sf
    where sf.year_no = year('2020');

```
