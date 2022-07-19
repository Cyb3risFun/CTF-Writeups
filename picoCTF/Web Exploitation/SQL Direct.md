# **_SQL Direct_**
## Description
Connect to this PostgreSQL server and find the flag!
## Solution
Connect to the `PostgreSQL` server with the given command
>psql -h saturn.picoctf.net -p 50525 -U postgres pico
Password is postgres

Notice the command shows that we are connecting to the `pico` database, list the tables with `\dt`
```console
pico=# \dt
         List of relations
 Schema | Name  | Type  |  Owner
--------+-------+-------+----------
 public | flags | table | postgres
(1 row)
```
There is a `flags` table, lets select all data from the table and get the flag
```
pico=# select * from flags;
 id | firstname | lastname  |                address
----+-----------+-----------+----------------------------------------
  1 | Luke      | Skywalker | picoCTF{L3arN_S0m3_5qL_t0d4Y_73b0678f}
  2 | Leia      | Organa    | Alderaan
  3 | Han       | Solo      | Corellia
(3 rows)
```