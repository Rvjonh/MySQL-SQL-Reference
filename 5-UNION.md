# UNION and UNION ALL

```SQL
    -- make a union of two tables
        -- select actors and customers first_name
    select a.first_name from actor a 
        union all 
    select c.first_name from customer c;

    -- make a union of two tables, ignoring duplicates like using distinct
    select a.first_name from actor a 
        union 
    select c.first_name from customer c;

    /* another example to not repeat equal items */
    select a.first_name from actor a 
        WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
    union
    select c.first_name from customer c
        WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%';
    
    -- difference with all
    select a.first_name, a.last_name from actor a 
        WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
    union all
    select c.first_name, c.last_name from customer c
        WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%';


    /*--------------------------------------------------------*/
    /*---------------------INTERSECT--------------------------*/
    /*----------------NOT SUPPORTED IN MYSQL------------------*/
    
    select a.first_name, a.last_name from actor a 
        WHERE a.first_name LIKE 'J%' AND a.last_name LIKE 'D%'
    INTERSECT
    select c.first_name, c.last_name from customer c
        WHERE c.first_name LIKE 'J%' AND c.last_name LIKE 'D%';
    
```

```SQL
    /*--------------------------------------------------------*/
    /*---------------------Examples--------------------------*/
    /*-------------------------------------------------------*/

    /*

    A = {L M N O P}
    B = {P Q R S T}

    • A union B -> {L M N O P Q R S T}
    • A union all B -> {L M N O P P Q R S T}
    • A intersect B -> {P}
    • A except B -> {L M N O} from b quit items in A

    */
```
