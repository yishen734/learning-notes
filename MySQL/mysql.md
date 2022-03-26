# MySQL

### Basics 
```sql
USE sql_store;
SELECT *
FROM customers 
WHERE state = ‘CA’
ORDER BY first_name
LIMIT 3; 
```

- SQL is not a case-sensitive language.
- In MySQL, every statement must be terminated with a semicolon.


### Comments 
We use comments to add notes to our code. 
```sql
-- This is a comment and it won’t get executed.
```

### Using Expressions
```sql
SELECT (points * 10 + 20) AS discount_factor
FROM customers 
```

#### Order of operations:
1. Parenthesis 
2. Multiplication / division 
3. Addition / subtraction 

#### Removing duplicates (DISTINCT)
```sql
SELECT DISTINCT state
FROM customers
```

### WHERE 
We use the WHERE clause to filter data. <br>
Comparison operators: 
- Greater than: ```>```
- Greater than or equal to: ```>=```
- Less than: ```<```
- Less than or equal to: ```<=``` 
- Equal: ```=``` 
- Not equal: ```<>``` 
- Not equal: ```!=```

### Logical Operators 
1. AND (both conditions must be True)
```sql
SELECT *
FROM customers 
WHERE birthdate > '1990-01-01' AND points > 1000
```
2. OR (at least one condition must be True)
```sql
SELECT *
FROM customers 
WHERE birthdate > '1990-01-01' OR points > 1000
```
3. NOT (to negate a condition)
```sql
SELECT *
FROM customers 
WHERE NOT (birthdate > '1990-01-01')
```

### IN Operator 
Returns customers in any of these states: VA, NY, CA
```sql
SELECT *
FROM customers 
WHERE state IN ('VA', 'NY', 'CA')
```

### BETWEEN Operator
```sql
SELECT *
FROM customers 
WHERE points BETWEEN 100 AND 200
```

### LIKE Operator 
Returns customers whose first name starts with 'b'
```sql
SELECT *
FROM customers 
WHERE first_name LIKE 'b%'
```
- ```%```: any number of characters 
- ```_```: exactly one character

### REGEXP Operator 
Returns customers whose first name starts with 'a'
```sql
SELECT *
FROM customers 
WHERE first_name REGEXP '^a'
```
- ```^```: beginning of a string 
- ```$```: end of a string 
- ```|```: logical OR 
- ```[abc]```: match any single characters 
- ```[a-d]```: any characters from a to d

#### More Examples 
1. Returns customers whose first name ends with 'EY' or 'ON'
```sql
WHERE first_name REGEXP 'ey$|on$'
```

2. Returns customers whose first name starts with 'MY' or contains 'SE'
```sql
WHERE first_name REGEXP ‘^my|se’
```

3. Returns customers whose first name contains B followed by 'R' or 'U'
```sql
WHERE first_name REGEXP 'b[ru]'
```

### IS NULL Operator 
Returns customers who don't have a phone number
```sql
SELECT *
FROM customers 
WHERE phone IS NULL
```

### ORDER BY Clause 
Sort customers by state (in ascending order), and then by their first name (in descending order)
```sql
SELECT *
FROM customers 
ORDER BY state, first_name DESC
```

### LIMIT Clause
Return only 3 customers 
```sql
SELECT *
FROM customers 
LIMIT 3
```

Skip 6 customers and return 3
```sql
SELECT *
FROM customers 
LIMIT 6, 3
```
