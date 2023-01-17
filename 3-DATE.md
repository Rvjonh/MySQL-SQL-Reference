# DATE

Notes for managing dates

```SQL
    -- returns current time, yes the three
    select now();
    select sysdate();

    -- used to make subseconds more precise ... '2023-01-13 13:55:22.852'
    select now(3);

    -- it's posible convert from UTF to unix timestamp ... and viseversa
    select FROM_UNIXTIME(1478960868932 * 0.001);

    -- returns the month of a date
    select month(current_date());

    -- extract pieze of date from a date
    SELECT EXTRACT(MONTH FROM CURRENT_DATE());

    -- extract the year from the date
    SELECT extract(YEAR FROM rental_date) year
    FROM rental;

```
