## SQL CLASS
----
### WHERE

The WHERE clause is used to filter records based on a specific condition.
````sql
SELECT *
FROM users
WHERE age > 18;
````
This example returns all users whose age is greater than 18.


## LIKE

The LIKE operator is used to search for patterns in text.

Common patterns:

A% (Starts with A) → Finds values that start with "A"
````sql
SELECT *
FROM users
WHERE name LIKE 'A%';
````
%A (Ends with A) → Finds values that end with "A"
````sql
SELECT *
FROM users
WHERE name LIKE '%a';
````
%A% (Contains A) → Finds values that contain "A" anywhere
````sql
SELECT *
FROM users
WHERE name LIKE '%a%';
````
## BETWEEN

The BETWEEN operator is used with WHERE to filter results within a range.
````sql
SELECT *
FROM products
WHERE price BETWEEN 10 AND 50;
````
Returns products with prices between 10 and 50 (inclusive).

## ORDER BY

The ORDER BY clause is used to sort query results.

Ascending (default):
````sql
SELECT *
FROM users
ORDER BY name ASC;
````
Descending:
````sql
SELECT *
FROM users
ORDER BY name DESC;
````
You can sort by one or multiple columns.
