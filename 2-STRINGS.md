# STRIGS

here are some notes from managing chars, varchar or strings in MySQL

```SQL
    -- shows the character configuration in the system
    select @@session.sql_mode;

    -- set a proper character system
    set sql_mode='ansi';

    -- put ansi mode, to truncate strings ... in insertions
    select @@session.sql_mode;

    -- returns the new string like, it would be store in the column
    select cast('hola como estas' AS VARCHAR(10));

```

```SQL
    -- Example to see the different deal with string data types
    create database Strings;
    use Strings;

    create table string_types (
        char_type CHAR(30),
        varchar_type varchar(30),
        text_type text
    );

    insert into string_types (char_type, varchar_type, text_type ) 
        values 
        ("hola como estas!",
        "hola como estan la gente linda de america latina",
        "hola como estan la gente linda de america latina");

    INSERT INTO string_types (char_type , varchar_type, text_type)
        VALUES ('This is char data',
        "This is varchar data alow\'wssdsd ", -- insert ' sign
        'This is text data');

    -- update text, in case be bigger will be truncate it
    UPDATE string_types 
        SET char_type = 'This is a piece of extremely long varchar data'
    WHERE char_type = 'This is char data';
```

## Especial functions in STRINGS

```SQL

    -- returns the text in quotes ""
    select quote(varchar_type) 'VARCHAR' from string_types;

    /* GENERATE CHARACTERS */
    SELECT 'abcdefg', CHAR(97,98,99,100,101,102,103);
    SELECT CHAR(99);

    -- add all strings in one, for a column
    SELECT CONCAT('danke sch', CHAR(148), 'n');

    -- repeats the string n times
    select REPEAT("hola ", 3);

    -- LOOK FOR SPECIAL CHARACTERS
    SELECT ascii('ö');

    -- get the lenght of a string
    select s.char_type, length(s.char_type) as 'length',
        s.varchar_type, length(s.varchar_type) as 'length',
        s.text_type, length(s.text_type) as 'length' 
        from string_types as s;

    -- looks for 'alow' in the column
    select position('alow' in s.varchar_type), s.varchar_type 
    from string_types as s;

    -- EXAMPLE
    select concat(c.first_name, ' ', c.last_name) as 'Name',
        concat('Registered from ', c.create_date) as 'Register'
        from customer as c;

    -- inserts string in another, with the option of substitute additional chars
    select insert('goodbye world', 1,7,'hello') as 'string';


    -- string to date
    select concat(c.first_name, ' ', c.last_name) as 'Name',
        str_to_date(c.create_date, '%d %M %Y') from customer as c;

    -- substract string from a string
    SELECT SUBSTRING('Please find the substring in this string', 17, 9);

```
