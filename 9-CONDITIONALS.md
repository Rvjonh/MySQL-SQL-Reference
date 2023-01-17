# CONDITIONALS LOGIG

> Create conditions based on cases

```SQL
    -- can put condifional in columns to show different data
    select  c.first_name,
            c.last_name, 
            case 
                when c.active = 1 then 'ACTIVE'
                else 'INACTIVE'
            end "Activity_type"
    from customer as c;

    -- retuns the numbers of rentals if the customer is actived
    select  c.first_name,
            c.last_name,
            case
                when c.active = 0 then 0
                else (select count(*) nums_rentals from rental as r where c.customer_id = r.customer_id)
            end customer_rentals
    from customer as c;

    -- exercices
        -- multiple case statements
    select name, 
        case name
            when 'English' then 'latin1'
            when 'Italian' then 'latin1'
            when 'French' then 'latin1'
            when 'German' then 'latin1'
            when 'Japanese' then 'utf8'
            when 'Mandarin' then 'utf8'
            else 'unknown'
        end character_set
    from language;

    -- available use other opertors like in substrings    
    select name, 
            case 
                when name in ('English', 'Italian', 'French', 'German') then 'latin1'
                when name in ('Japanese', 'Mandarin') then 'utf8'
                else 'Unknown'
            end character_set
    from language;

    -- conditional en aggregate function
        -- returns the count of every film by rating
    select  sum(case when f.rating = 'G' then 1 else 0 end) G,
            sum(case when f.rating = 'PG' then 1 else 0 end) PG,
            sum(case when f.rating = 'PG-13' then 1 else 0 end) PG_13,
            sum(case when f.rating = 'R' then 1 else 0 end) R,
            sum(case when f.rating = 'NC-17' then 1 else 0 end) NC_17
    from film as f;
```

## CASE or IF

it's almost the same aspect, a conditional but:
    - case can have multiple when ...
    - if have a condition, and to answers, one if it's true and another if it's false

```SQL

    -- example
    select first_name, note,
        case when note >= 65 then 'pass' else 'fail' end as 'Remark'
    from students;
    -- with the 'if'
    select first_name, note,
        if (note >= 65, 'pass', 'fail') as 'Remark'
    from students;

```

## UPDATING WITH CONDITIONAL

```SQL

    update payment 
        set amount = amount + 
            case staff_id
                when 1 then 1
                when 2 then 2
                else 0
            end
    where staff_id in (1 , 2);

```
