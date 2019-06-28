# SQL Generalizations
SQL is a programming language designed to manipulate and manage data stored in relational databases.
- A relational database is a database that organizes information into one or more tables.
- A table is a collection of data organized into rows and columns.
A statement is a string of characters that the database recognizes as a valid command.
## Six Commands Commonly Used:
- `CREATE`: Creates new table
- `INSERT INTO`: adds a new row to a table
- `SELECT`: queries data from a table.
- `ALTER TABLE`: changes an existing table.
- `UPDATE`: edits a row in a table.
- `DELETE FROM`: deletes rows from a table.

## Common data types in SQL:
- Integer
- Text
- Data
- Real

## Six Common Statements in SQL:
### 1.CREATE
`CREATE TABLE table_name(
column_name_1 data_type,
column_name_2 data_type,
column_name_3 data_type);`

### 2.INSERT
`INSERT INTO  table_name(
column_name_1,column_name_2,column_3)
VALUES  (a1,b1,c1),(a2,b2,c2),(a3,b3,c3);`

### 3.SELECT
`SELECT * FROM table_name;`,`SELECT column_name FROM table_name;`

### 4.ALTER
`ALTER TABLE table_name ADD COLUMN new_column_name TEXT;`

### 5.UPDATE
`UPDATE table_name SET (to_be_modified) WHERE (loc_conditon);`

### 6.DELETE
`DELETE FROM table_name WHERE (condition);`

### Constrains
Constraints that add information about how a column can be used are invoked after specifying the data type for a column. 
They can be used to tell the database to reject inserted data that does not adhere to a certain restriction. For example:
- `CREATE TABLE table_name(
   column_name_1 INTEGER PRIMARY KEY, 
   column_name_2 TEXT UNIQUE,
   column_name_3 TEXT NOT NULL,
   column_name_$ TEXT DEFAULT 'Not Applicable'
);`
We found that the constrains words like `PRIMARY KEY`, `UNIQUE`, `NOT NULL`, `DEFAULT (VALUE)`.

## Chapter 1: Writing Queries
One of the core purposes of the SQL language is to retrieve information stored in a database. 
This is commonly referred to as querying. Queries allow us to communicate with the database by asking questions and having the result set return data relevant to the question.

Reviews:
- `SELECT` is the clause we use every time we want to query information from a database.
- `AS` renames a column or table.
- `DISTINCT` return unique values.
- `WHERE` is a popular command that lets you filter the results of the query based on conditions that you specify.
- `LIKE` and `BETWEEN` are special operators.
- `AND` and `OR` combines multiple conditions.
- `ORDER BY` sorts the result.
- `LIMIT` specifies the maximum number of rows that the query will return.
- `CASE` creates different outputs.
### AS
`AS` is a keyword in SQL that allows you to rename a column or table using an alias. Some important things to note:
- Although it’s not always necessary, it’s best practice to surround your aliases with single quotes.
- When using `AS`, the columns are not been renamed in the table. The aliases only appear in the result.

### DISTINCT
`DISTINCT` is used to return unique values in the output. It filters out all duplicate values in the specified column(s).

### WHERE
DISTINCT is used to return unique values in the output. It filters out all duplicate values in the specified column(s).
Comparison operators used with the WHERE clause are:
- `=` equal to
- `!=` not equal to
- `>` greater than
- `<` less than
- `>=` greater than or equal to
- `<=` less than or equal to
### LIKE
LIKE can be a useful operator when you want to compare similar values.
- The `_` means you can substitute any individual character here without breaking the pattern.For example: 
  - The names Seven and Se7en both match the pattern:`LIKE 'Se_ev'`.
- The `%` is a wildcard character that matches zero or more missing letters in the pattern.For example:
  - `LIKE 'A%'` matches all words with names that begin with letter ‘A’
  - `LIKE '%a'` matches all words that end with ‘a’.
  - We can also use % both before and after a pattern:`LIKE '%xxx%'`
  
### IS NULL
More often than not, the data you encounter will have missing values.we will have to use these operators:
- `IS NULL`
- `IS NOT NULL`

### BETWEEN
The `BETWEEN` operator can be used in a WHERE clause to filter the result set within a certain range. 
The values can be numbers, text or dates.interesting point to emphasize:
- `BETWEEN` two letters is not inclusive of the 2nd letter.For example:
  - `SELECT * FROM movies WHERE name BETWEEN 'A' AND 'J';`The results merely return a series of movie name with a letter of 'A'-'I'.
- `BETWEEN` two numbers is inclusive of the 2nd number.For example:
  - `SELECT * FROM movies WHERE year BETWEEN 1990 AND 1999;`The results return with the year between 1990 and 1999.
  
### AND
Sometimes we want to combine multiple conditions in a `WHERE` clause to make the result set more specific and useful.
With `AND`, both conditions must be true for the row to be included in the result.

### OR
Similar to `AND`, the `OR` operator can also be used to combine multiple conditions in `WHERE`
With `OR`, if any of the conditions are true, then the row is added to the result.

### ORDER BY (DESC)
`ORDER BY` is a clause that indicates you want to sort the result set by a particular column.(can have more than one column)
The column that we `ORDER BY` doesn’t even have to be one of the columns that we’re displaying.

### LIMIT
`LIMIT` is a clause that lets US specify the maximum number of rows the result set will have. 
This saves space on our screen and makes our queries run faster.
Here, we specify that the result set can’t have more than 10 rows.
`LIMIT` always goes at the very end of the query. Also, it is not supported in all SQL databases.

### CASE
A CASE statement allows us to create different outputs (usually in the SELECT statement). 
It is SQL’s way of handling if-then logic.For example:
- `SELECT name,CASE WHEN genre='romance' THEN 'Chill'
WHEN genre='comedy' THEN 'Chill'
ELSE 'Intense'
END AS 'Mood'
From movies;`

## Chapter 2: Aggregate Functions
Calculations performed on multiple rows of a table are called aggregates.

Reviews:
- `COUNT()`: count the number of rows
- `SUM()`: the sum of the values in a column
- `MAX()/MIN()`: the largest/smallest value
- `AVG()`: the average of the values in a column
- `ROUND()`: round the values in the column
- `GROUP BY` is a clause used with aggregate functions to combine data from one or more columns.
- `HAVING` limit the results of a query based on an aggregate property.

### COUNT
`COUNT()` is a function that takes the name of a column as an argument and counts the number of non-empty values in that column.

### SUM
`SUM()` is a function that takes the name of a column as an argument and returns the sum of all the values in that column.

### MAX/MIN
The `MAX()` and `MIN()` functions return the highest and lowest values in a column, respectively.

### AVG()
The `AVG()` function works by taking a column name as an argument and returns the average value for that column.

### ROUND()
`ROUND()` function takes two arguments inside the parenthesis:
- A column name
- A column name

### GROUP BY
`GROUP BY` is a clause in SQL that is used with aggregate functions. It is used in collaboration with the SELECT statement to arrange identical data into groups.The GROUP BY statement comes after any WHERE statements, but before ORDER BY or LIMIT.
SQL lets us use column reference(s) in our GROUP BY that will make our lives easier:
- `1` is the first column selected
- `2` is the second column selected
- `3` is the third column selected

### HAVING
`HAVING` allows you to filter which groups to include and which to exclude.
`HAVING` statement always comes after GROUP BY, but before ORDER BY and LIMIT.

## Chapter 3: Working With Multiple Tables
In order to efficiently store data, we often spread related information across multiple tables.Such as:
- the `orders` would contain just the information necessary to describe what was ordered:
   - order_id, customer_id, subscription_id, purchase_date
- subscriptions would contain the information to describe each type of subscription:
   - subscription_id, description, price_per_month, subscription_length
- customers would contain the information for each customer:
   - customer_id, customer_name, address
   
Reviews:
- `JOIN` will combine rows from different tables if the join condition is true.
- `LEFT JOIN` will return every row in the left table, and if the join condition is not met, NULL values are used to fill in the columns from the right table.
- Primary key is a column that serves a unique identifier for the rows in the table.
- Foreign key is a column that contains the primary key to another table.
- `CROSS JOIN` lets us combine all rows of one table with all rows of another table.
- `UNION` stacks one dataset on top of another.
- `WITH` allows us to define one or more temporary tables that can be used in the final query.

### JOIN: `SELECT...FROM...JOIN...ON...;`
Because column names are often repeated across multiple tables, we use the syntax table_name.column_name to be sure that our requests for columns are unambiguous. In our example, we use this syntax in the ON statement, but we will also use it in the SELECT or any other statement where we refer to column names.

### INNER JOIN: `SELECT...FROM...INNER JOIN...ON...;`
When we perform a simple `JOIN` (often called an `inner join`) our result only includes rows that match our ON condition.

### LEFT JOIN: `SELECT...FROM...LEFT JOIN...ON...;`
A `left join` will keep all rows from the first table, regardless of whether there is a matching row in the second table.
The final result will keep all rows of the left table but will omit the un-matched row.

### PRIMARY KEY vs FOREIGN KEY
Each of tables has a column that uniquely identifies each row of that table, for example:
- `order_id` for `orders`
- `subscription_id` for `subscriptions`
- `customer_id` for `customers`
These special columns are called primary keys.Primary keys have a few requirements:
- None of the values can be NULL.
- Each value must be unique (i.e., you can’t have two customers with the same customer_id in the customers table).
- A table can not have more than one primary key column.
When the primary key for one table appears in a different table, it is called a foreign key.
- So `customer_id` is a primary key when it appears in `customers`, but a foreign key when it appears in `orders`.
The most common types of joins will be joining a foreign key from one table with the primary key from another table. For instance:
- When we join `orders` and `customers`, we join on `customer_id`, which is a foreign key in `orders` and the primary key in `customers`.

### CROSS JOIN: `SELECT...FROM...CROSS JOIN...;`
Combining all rows of one table with all rows of another table.
Notice that `cross joins` don’t require an ON statement. You’re not really joining on any columns!
Suppose we have 3 different shirts (white, grey, and olive) and 2 different pants (light denim and black), the results might look like this: `3 shirts × 2 pants = 6 combinations`
- A more common usage of CROSS JOIN is when we need to compare each row of a table to a list of values.

### UNION: `SELECT...FROM...UNION SELECT...FROM...;`
Sometimes we just want to stack one dataset on top of the other. Well, the `UNION` operator allows us to do that.
SQL has strict rules for appending data:
- Tables must have the same number of columns.
- The columns must have the same data types in the same order as the first table.

### WITH: `WITH...AS(SELECT.......)SELECT...FROM...JOIN...ON...;`
Often times, we want to combine two tables, but one of the tables is the result of another calculation.
- The `WITH` statement allows us to perform a separate query (such as aggregating customer’s subscriptions)
- `previous_results` is the alias that we will use to reference any columns from the query inside of the WITH clause
- We can then go on to do whatever we want with this temporary table (such as join the temporary table with another table)
Essentially, we are putting a whole first query inside the parentheses () and giving it a name. After that, we can use this name as if it’s a table and write a new query using the first query.
- `With previous_results_name[a temporary table using aggregation] AS(SELECT.....) SELECT ....FROM...JOIN...ON ...=...;`
