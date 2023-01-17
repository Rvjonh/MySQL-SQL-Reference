# MATH FUNCTIONS

```SQL

    -- absolute number
    select abs(−25,76823) AS 'ABS';

    -- returns the symbol of a number
        -- (-1) if it's negative
        -- (1) if it's positive
        -- (0) if it's zero
    select SIGN(-25.76823);

    -- round number
    select abs(−25,76823) AS 'ABS',
            round(−25,76823) as 'ROUND';

    -- returns the power of a number pow(n,m)
        -- n base
        -- m exponent
    select pow(2,2);

    -- numbers to binary
    select bin(10);

```

## Numeric Data Types

Casting numbers, numeric decimals

```SQL
    select cast(123  as decimal(5,2));
    select cast(12345 as decimal(10,5));

    select cast(pi() as float);
    select cast(pi() as real);
```

> There are data values for money, which are 'money' and 'smallmoney', not supported in MySql

```SQL
    -- Binaries
    select cast(12345 as binary(10));
```
