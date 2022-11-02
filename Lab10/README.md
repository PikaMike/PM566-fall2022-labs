PM566-lab10-Tiansheng-Jin
================
Tiansheng Jin
2022-11-02

``` r
# install.packages(c("RSQLite", "DBI"))
# if (!require(RSQLite)) install.packages(c("RSQLite"))
# if (!require(DBI)) install.packages(c("DBI"))

library(RSQLite)
library(DBI)

# Initialize a temporary in memory database
con <- dbConnect(SQLite(), ":memory:")

# Download tables
actor <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/actor.csv")
rental <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/rental.csv")
customer <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/customer.csv")
payment <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/payment_p2007_01.csv")

# Copy data.frames to database
dbWriteTable(con, "actor", actor)
dbWriteTable(con, "rental", rental)
dbWriteTable(con, "customer", customer)
dbWriteTable(con, "payment", payment)

dbListTables(con)
```

    ## [1] "actor"    "customer" "payment"  "rental"

``` sql
PRAGMA table_info(actor)
```

| cid | name        | type    | notnull | dflt_value |  pk |
|:----|:------------|:--------|--------:|:-----------|----:|
| 0   | actor_id    | INTEGER |       0 | NA         |   0 |
| 1   | first_name  | TEXT    |       0 | NA         |   0 |
| 2   | last_name   | TEXT    |       0 | NA         |   0 |
| 3   | last_update | TEXT    |       0 | NA         |   0 |

4 records

``` r
dbGetQuery(con,
           "PRAGMA table_info(actor)")
```

    ##   cid        name    type notnull dflt_value pk
    ## 1   0    actor_id INTEGER       0         NA  0
    ## 2   1  first_name    TEXT       0         NA  0
    ## 3   2   last_name    TEXT       0         NA  0
    ## 4   3 last_update    TEXT       0         NA  0

# Exercise 1

Retrive the actor ID, first name and last name for all actors using the
actor table. Sort by last name and then by first name.

``` r
dbGetQuery(con, "
 SELECT actor_id, first_name, last_name
 FROM actor
 ORDER by last_name, first_name
 LIMIT 10
             ")
```

    ##    actor_id first_name last_name
    ## 1        58  CHRISTIAN    AKROYD
    ## 2       182     DEBBIE    AKROYD
    ## 3        92    KIRSTEN    AKROYD
    ## 4       118       CUBA     ALLEN
    ## 5       145        KIM     ALLEN
    ## 6       194      MERYL     ALLEN
    ## 7        76   ANGELINA   ASTAIRE
    ## 8       112    RUSSELL    BACALL
    ## 9       190     AUDREY    BAILEY
    ## 10       67    JESSICA    BAILEY

Try in SQL

``` sql
SELECT actor_id, first_name, last_name
FROM actor
ORDER by last_name, first_name
```

| actor_id | first_name | last_name |
|---------:|:-----------|:----------|
|       58 | CHRISTIAN  | AKROYD    |
|      182 | DEBBIE     | AKROYD    |
|       92 | KIRSTEN    | AKROYD    |
|      118 | CUBA       | ALLEN     |
|      145 | KIM        | ALLEN     |
|      194 | MERYL      | ALLEN     |
|       76 | ANGELINA   | ASTAIRE   |
|      112 | RUSSELL    | BACALL    |
|      190 | AUDREY     | BAILEY    |
|       67 | JESSICA    | BAILEY    |

Displaying records 1 - 10

# Exercise 2

Retrive the actor ID, first name, and last name for actors whose last
name equals ‘WILLIAMS’ or ‘DAVIS’.

``` sql
SELECT actor_id, first_name, last_name
FROM actor
WHERE last_name IN ('WILLIAMS', 'DAVIS')
ORDER by last_name
```

| actor_id | first_name | last_name |
|---------:|:-----------|:----------|
|        4 | JENNIFER   | DAVIS     |
|      101 | SUSAN      | DAVIS     |
|      110 | SUSAN      | DAVIS     |
|       72 | SEAN       | WILLIAMS  |
|      137 | MORGAN     | WILLIAMS  |
|      172 | GROUCHO    | WILLIAMS  |

6 records

# Exercise 3

Write a query against the rental table that returns the IDs of the
customers who rented a film on July 5, 2005 (use the rental.rental_date
column, and you can use the date() function to ignore the time
component). Include a single row for each distinct customer ID.

SELECT DISTINCT FROM WHERE date(\_\_\_) = ‘2005-07-05’

# Exercise 4

Exercise 4.1 Construct a query that retrives all rows from the payment
table where the amount is either 1.99, 7.99, 9.99.

SELECT \* FROM *** WHERE *** IN (1.99, 7.99, 9.99) Exercise 4.2
Construct a query that retrives all rows from the payment table where
the amount is greater then 5

SELECT \* FROM WHERE Exercise 4.2 Construct a query that retrives all
rows from the payment table where the amount is greater then 5 and less
then 8

SELECT \* FROM *** WHERE *** AND \_\_\_ \# Exercise 5 Retrive all the
payment IDs and their amount from the customers whose last name is
‘DAVIS’.

SELECT FROM INNER JOIN WHERE AND \# Exercise 6 Exercise 6.1 Use
COUNT(\*) to count the number of rows in rental

Exercise 6.2 Use COUNT(\*) and GROUP BY to count the number of rentals
for each customer_id

Exercise 6.3 Repeat the previous query and sort by the count in
descending order

Exercise 6.4 Repeat the previous query but use HAVING to only keep the
groups with 40 or more.

# Exercise 7

The following query calculates a number of summary statistics for the
payment table using MAX, MIN, AVG and SUM

Exercise 7.1 Modify the above query to do those calculations for each
customer_id

Exercise 7.2 Modify the above query to only keep the customer_ids that
have more then 5 payments

# Cleanup

Run the following chunk to disconnect from the connection.

``` r
# clean up
dbDisconnect(con)
```
