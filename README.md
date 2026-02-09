# PostgreSQL (@MPrashantTech)

- World's most Advance, Opensource, Relational Database.

## Quick Look ups:
- Server: localhost
- Database: postgres
- Port: 5432
- username: postgres
- it's case insensitive but uppercase makes it more readable.

## Overview
- Basics
- CRUD 
- Aggregate functions and Operators
- Relationship & it's types (vvi) 
- Joins & It's types
- Advance Topics like (Stored Routines, it's types, Stored Procedures, User Defined functions, windows functions, Views, Triggers, CT)

## Understanding Basics:

#### Questions:
- What is database?
An organised collection of data. A method to manipulate and access the data. (eg., Library)

- Database vs DBMS
DBMS -> (eg., Apps have Database, DBMS like Postgres, MySQL helps to manage those database!)

- DBMS vs RDBMS
RDBMS -> A type of database system that stores data in structured tables (using rows and columns) and uses SQL for managing and querying data.

- Other available databases
Other Databases (used for different purpose) : 
```database
- MongoDB
- Oracle
- MySQL
- SQLite
- PostgreSQL
- MaxDB
- Firebird
- Redis
```

- SQL vs PostgreSQL
SQL is simply a language which is used to talk to our databases. (eg., SELECT * FROM users;)
Where, PostgreSQL is a tool (RDBMS) 

## Installation

## windows CLI
```post
After running the initial/ default commands in SQL Shell (psql), we'll see something like this:
postgres=#
commands: 
- \list (to check existing database)
- \! cls (clear)
- CREATE DATABASE users;
- \c database (to change)
- \dt (to desc table)
- \dt+ (more detailed)

for using commands in terminals like cmd or zsh:
- set bin folder to environment variables
- psql -U postgres (followed by password!)
- postgres=# (db_name=#)
```

## Database vs Schema vs Table

## Section - 2 (starts)

content:
- Database: creating, connect, listing, droping
- CRUD
        - Create - New table
            - Inserting data
        - Read - How to read data
        - Update Data
        - Delete Data

### Database:
#### 1. List down existing databases:
`SELECT datname FROM pg_database;`
\l or \list
- Initially, you'd see 3 databases postgres, template1 and template2 (these templates are by default hidden, that's why they don't appear on the pgadmin!

#### 2. Change a Database
`\c <db_name>;`

#### 3. Deleting a Database
`DROP DATABASE <db_name>;`

### CRUD:
#### 1. Create Table
Table: A table is a collection of related data help in a table format within a database.
```postgres
CREATE TABLE person (
    id INT, 
    name VARCHAR(100),
    city VARCHAR(100)
);

CREATE TABLE person (id INT, name VARCHAR(100), city VARCHAR(50));
```
#### 2. Desc table
`\d person;`

#### 3. Inserting into table
```psql
INSERT INTO person(id, name, city) VALUES (101, 'Raju', 'Delhi');

INSERT INTO students VALUES (101, "Rahul");
```

#### 4. Reading Data
```psql
SELECT * FROM person;
```

#### 5. Updating Data Table
```psql
UPDATE person 
SET city = 'London'
WHERE id=2;
```

#### 6. Deleting Data
```psql
DELETE FROM students
WHERE name = 'Raju'; // use id!
```

#### Problems:
1. When id is not unique
2. When one column value is not inserted and we get null
(we're gonna see how to handle these problems in next section!)

## Section 3:

```yaml
- Datatypes :
An attribute that specifies the type of data in a column of our database - table.
Most Widely used are:
    1. Numeric - INT DOUBLE FLOAT DECIMAL
    2. String - VARCHAR
    3. Date - DATE
    4. Boolean - BOOLEAN
(search Datatype postgress)
- DECIMAL(5, 2) or NUMERIC(5, 2) -> Digits, Precision (eg., 119.12 correct, 28.15 correct, 1150.15 wrong!)


- Constraint: A constraint in PostgreSQL is a rule applied to a column.
    - Primary Key:
        -  The PRIMARY KEY constraint uniquely identifies each record in a table.
        -  Primary keys must contain UNIQUE values, and cannot contain NULL  values.
        -  A table can have only ONE primary key.
    - NOT NULL:
        ```psql
            CREATE TABLE customers(
                id INT NOT NULL,
                name VARCHAR(100) NOT NULL
            );
        ```
    - Default:
        ```psql
            CREATE TABLE customers(
                acc_no INT PRIMARY KEY,
                name VARCHAR(100) NOT NULL,
                acc_type VARCHAR(50) NOT NULL DEFAULT 'Savings'
            ); 
            // we can make Bank app in Java using OOPs on this!
        ```

    - Auto_increment:
        ```psql
            // we use SERIAL for auto_increment in psql
            CREATE TABLE employees (
                id SERIAL PRIMARY KEY,
                firstname VARCHAR(50),
                lastname VARCHAR(50)
            );
        ```
    - Task : Create an employees table
    ```psql
        CREATE TABLE employees (
            emp_id SERIAL PRIMARY KEY,
            fname VARCHAR(50) NOT NULL,
            lname VARCHAR(50) NOT NULL,
            email VARCHAR(100) NOT NULL UNIQUE,
            dept VARCHAR(50),
            salary DECIMAL(10, 2) DEFAULT 30000.00,
            hire_date DATE NOT NULL DEFAULT CURRENT_DATE
        );

        SELECT currval('employees_emp_id_seq');
        SELECT setval('employees_emp_id_seq', 1); // if the initial value was set manually! 


    ```
Q: What are public schemas, private schemas??

    - Serial (done)
    - Unique (done)
```

## Section 4: (Data Refining)

```psql
common Clauses and it's uses:

Clause means conditions in sql queries;

- Where: 
eg. `SELECT * FROM employees WHERE dept = 'IT' AND salary <= 50000;`
Here, we have used Where Clause with Relational Operators, as well as Logical Operator! 

SELECT * FROM employees WHERE dept = 'IT' OR dept = 'HR' OR dept = 'Finance';

better way is to use IN, NOT IN 

SELECT * FROM employees WHERE dept IN ('IT', 'HR', 'Finance');

Between: SELECT * FROM employees WHERE salary BETWEEN 40000 AND 65000;

- Distinct:
Distinct means Unique values1 
eg., `SELECT DISTINCT fname FROM employees;`

eg., `SELECT dept FROM employees;` -> `SELECT DISTINCT dept FROM employees;`

- Order by: It is used for sorting of data!
eg., `SELECT * FROM employees ORDER BY fname;` 
sorting means alphabetical sorting!
eg., `SELECT * FROM employees ORDER BY fname DESC;`

- Limit
eg., `SELECT * FROM employees LIMIT 3;`

- Like: (For finding patterns)
eg., `SELECT * FROM employees WHERE dept LIKE '%Acc%';`
eg., `SELECT * FROM employees WHERE dept LIKE '__';` (dept with two char names like IT, HR )
eg., `SELECT * FROM employees WHERE fname LIKE '_a%';` (employee fname having 'a' as a second character!)

```


## Section - 5 (Aggegate Functions)
where do we need this??
answers:
- How to find total no. of employees?
- Employee with Max salary
- Average salary of employees

#### Functions:
- COUNT :
eg., `SELECT COUNT(emp_id) FROM employees;` (find the difference between using * and a column name!)

- SUM: 
eg., `SELECT SUM(salary) FROM employees;`

- AVG
eg., `SELECT AVG(salary) FROM employees;`

- MIN
eg., `SELECT MIN(salary) FROM employees;`

- MAX
eg., `SELECT MAX(salary) FROM employees;`

### What is GROUP BY? : (VVI for interviews and Data Analytics roles)

Q: No. of employees in each department
A: SELECT DISTINCT dept, COUNT(emp_id) FROM employees GROUP BY dept;

GROUP BY comes with AGGREGATE FUNCTIONS
Q: Does it always comes with aggregate functions? 
A: NO!
eg., 
SELECT customer_num
FROM orders
GROUP BY customer_num;

gives same result as :
SELECT DISTINCT customer_num
FROM orders;


## Section - 6: (String Functions)
It is used when we have a large table and we want to extract data and generate a report or simply print, then we need to do manipulation, decoration, some changes etc in the column's values, we use them!

```psql
- CONCAT, CONCAT_WS (with separator) :
CONCAT(first_col, sec_col)
CONCAT(first_word, sec_word, ...)
```psql
SELECT CONCAT('Hello', 'World');
SELECT CONCAT(fname, lname) AS Fullname FROM employees;
SELECT CONCAT(fname, ' ' , lname) AS Fullname FROM employees;

SELECT CONCAT_WS(':', 'One', 'Two); // or add space!

// one unique thing!
SELECT CONCAT_WS(' ', fname, lname, salary) AS Employee_Data FROM employees;
```

- SUBSTR
eg., `SELECT SUBSTRING('Hey Buddy', 1, 4);` // SUBSTRING OR SUBSTR
```psql
// one usecase example: when you want to take out country code from phone numbers! +91
// eg., 1
SELECT SUBSTR('Hello buddy!', 1, 5);
// output: Hello
```

- REPLACE

REPLACE: eg., Hey buddy : Hey -> Hello : Hello Buddy
```psql
REPLACE(str, from_str, to_str)
REPLACE('Hey Buddy', 'Hey', 'Hello')

eg., 1:

SELECT REPLACE('ABCXYZ', 'ABC', 'PQR');
// output: PQRXYZ

eg., 2:

SELECT REPLACE(dept, 'IT', 'TECH') from employees;
// i guess it only prints results, not update the actual data! Let's check! Yes! i was right!
```

Similarly, we have REVERSE() not very useful but good to know!
eg., `SELECT REVERSE('Hello');`

eg., `SELECT REVERSE(fname) FROM employees;`


- LENGTH:

Calculates the length of the provided string!
eg., `SELECT LENGTH('Hello World');`

SELECT LENGTH(fname) FROM employees;


- UPPER, LOWER: easy one! Mostly used for presentation!


- LEFT, RIGHT:
more useful than SUBSTR() , although, is the same!

eg., 1:
`SELECT LEFT('Hello World', 5);` 
// prints Hello

eg., 2: 
`SELECT RIGHT('Hello World', 5);` 
// prints World


- TRIM, LTRIM, RTRIM: 
usecase: password filtering!

eg., 1:
`SELECT LENGTH(TRIM('      Alright     ' ));`
output: 7

- POSITION: to find the position of a word or char!
eg., 1:
`SELECT POSITION('OM' in 'Thomas');`


- STRING_AG
```

Task 1:
 1:Raj:Sharma:IT
 ```psql
    bank_db=# SELECT
    bank_db-# CONCAT_WS(':', emp_id, fname, lname, dept)
    bank_db-# FROM employees
    bank_db-# WHERE
    bank_db-# emp_id = 1;
 ```

Task 2: 
 1:Raj Sharma:IT:50000.00
```psql
    SELECT
    CONCAT_WS(':', emp_id, CONCAT_WS(fname, lname), dept, salary)
    FROM employees
    WHERE
    emp_id=1;
```

Task 3:
 4:Suman:FINANCE
 ```psql
    bank_db=# SELECT 
    CONCAT_WS(':',emp_id, fname, UPPER(dept))
    FROM employees
    WHERE
    emp_id=4;
 ```

 Task 4:
  I1 Raj
 H2 Priya
```psql
    bank_db=# SELECT
    bank_db-# CONCAT_WS(' ', CONCAT(LEFT(dept, 1), emp_id), fname)
    bank_db-# FROM employees
    bank_db-# WHERE 
    bank_db-# emp_id=1 OR emp_id=2;
```


More Exercise:

[ EX1: 
DISTINCT, ORDER BY, LIKE and LIMIT

```text
These are the types of questions you can expect in interviews!

1. Find Different type of departments in database?
2. Display records with High-low salary
3. How to see only top 3 records from a table?
4. Show records where first name start with letter 'A'
5. Show records where length of the lname is 4 character
```

]

[ EX2:
COUNT, GROUP BY, MIN, MAX, and SUM and AVG

```text
1. Find Total no. of employees in database?
2. Find no. of employees in each department.
3. Find lowest salary paying
4. Find highest salary paying (cool question!)
way 1 (by me) -> SELECT * FROM employees WHERE salary=(SELECT MAX(salary) FROM employees); 
way 2 (by tutor) -> SELECT * FROM employees ORDER BY salary DESC LIMIT 1; 
Tutor said way 2 doesn't handle multiple users with max salary!
5. Find total salary paying in Loan department?
6. Average salary paying in each department
```
]


## Section 7: ALTER Query

For this section, we'll use the person table!
eg., 1 : Add column
`ALTER TABLE users ADD COLUMN age INT;` // we can use default here! well how to add multiple columns??

eg., 2 : Drop column
`ALTER TABLE users DROP COLUMN age;`

eg., 3 : Rename column
`ALTER TABLE users RENAME COLUMN name TO fname;`

eg., 4 : Change Table name
`ALTER TABLE users RENAME TO new_users;`
or simply
`ALTER TABLE users TO new_users;`

eg., 5 : Change column Data type
`ALTER TABLE users ALTER COLUMN fname SET DATA TYPE VARCHAR(130);`

`ALTER TABLE users ALTER COLUMN fname SET DEFAULT 'unknown' ;`

`ALTER TABLE users ALTER COLUMN fname SET NOT NULL;` // we can also use DROP here!!

### Constraint CHECK (an additional topic!)
We'll combined ALTER and CHECK
usecase: We want to make sure phone no. is atleast 10 digits...
eg., `CREATE TABLE contancts(
            name VARCHAR(50),
            mob VARCHAR(15) UNIQUE CHECK (LENGTH(mob) >= 10)
);`

eg., `ALTER TABLE person
        ADD COLUMN
            mob VARCHAR(15)
                CHECK (LENGTH(mob) >= 10);`
```psql

    // check :
    \d person;
    // you'll see the name of our constraint: person_mob_check
```

```psql
ALTER TABLE contacts
DROP CONSTRAINT mob_no_less_than_10digits;

ALTER TABLE contacts
ADD CONSTRAINT mob_not_null CHECK (mob != null);

// Named Constraint
CREATE TABLE contacts(
    name VARCHAR(50), 
    mob VARCHAR(15) UNIQUE,
    CONSTRAINT mob_no_less_than_10digits CHECK (LENGTH(mob) >= 10)
);

// The usecase of this is to use it in logs to define errors! (for more readability / understandability!) 
// that is, mob_no_less_than_10digits

```

### Expression CASE
usecase: in employee table, you are required to print fname, salary, Salary Category (low, higher, mid, etc)
more usecase: grade of students, marks -> fail / pass
- if else condition like logic (you can even use switch)
- CASES are used for representation purpose (presentation) , so you can make new columns to display desired results

```psql

// eg., 1 : 
SELECT fname, salary,
CASE
        WHEN salary >= 50000 THEN 'High'
        ELSE 'Low'
END AS sal_cat
FROM
        employees;
```

```psql

// eg., 2 : we can even add multiple cases!
SELECT fname, salary, 
CASE
        WHEN salary >= 50000 THEN 'HIGH'
        WHEN salary >= 40000 AND
                    salary < 50000 THEN 'MID'
        ELSE 'LOW'
END AS sal_cat
FROM 
        employees;

```

```psql
//eg., 3 : same as eg 2, just with BETWEEN

SELECT fname, salary, 
CASE 
        WHEN salary >= 50000 THEN 'HIGH'
        WHEN salary BETWEEN 45000 AND 50000
                    THEN 'MID'
        ELSE 'LOW'
END AS sal_cat
    FROM employees;
```

```txt

Task 1 : print bonus

 fname  |  salary  | bonus 
--------+----------+-------
 Raj    | 50000.00 |  5000
 Priya  | 45000.00 |  4500
 Arjun  | 55000.00 |  5500
 Suman  | 60000.00 |  6000
 Kavita | 47000.00 |  4700
 Amit   | 52000.00 |  5200
 Neha   | 48000.00 |  4800
 Rahul  | 53000.00 |  5300
 Anjali | 61000.00 |  6100
 Vijay  | 50000.00 |  5000

Query : SELECT fname, salary, CASE WHEN salary > 0 THEN Round(salary*.10) END AS bonus FROM employees;


--------------------------------------------------------------------------------------------------

Task 2 : print this -> 
 bonus | count 
-------+-------
 High  |     5
 Mid   |     3
 Low   |     2

 Query : SELECT CASE WHEN salary > 50000 THEN 'High' WHEN salary BETWEEN 48000 AND 50000 THEN 'Mid' ELSE 'Low' END AS bonus, COUNT(emp_id) FROM employees GROUP BY bonus;

 SubQuery : SELECT bonus, COUNT(emp_id)
FROM (
    SELECT emp_id, 'High' AS bonus
    FROM employees
    WHERE salary > 50000

    UNION ALL

    SELECT emp_id, 'Mid' AS bonus
    FROM employees
    WHERE salary BETWEEN 48000 AND 50000

    UNION ALL

    SELECT emp_id, 'Low' AS bonus
    FROM employees
    WHERE salary < 48000
) AS t
GROUP BY bonus;


```

## Section 8 : RELATIONSHIP

- In real life, companies database looks like Salary, Attendence, Employees, Requests, Offices, Task etc, not just Employees! 

- The more distributed the database is , the more it's easier to maintain!
- The Relationship is basically how these tables are connected to each others!
- Types of Relationship: 
        - One to One (employee to employee_details)
        - One to Many (employee to employee_task)
        - Many to Many (books to authors)
- Why do we design a good database? So, that debugging, maintainance is easy, as well as it is open for doing analysis and making predictions or business regarding decisions

#### Foreign Key : 
A foreign key is a column or set of columns in one relational database table that provides a link between data in two tables by referencing the primary key of another table.

For Foreign key and Relationship understanding, we'll make another database : storedb and two tables customers and orders;

### Understanding JOINS 

Def : JOIN operation is used to combine rows from two or more tables based on a related column between them. eg., customers and orders

Types of Join: 
    ◆ Cross Join
    ◆ Inner Join
    ◆ Left Join
    ◆ Right Join

1. Cross Join : Every row from one table is combained with every row from another table.
eg., `SELECT * FROM customers CROSS JOIN orders;`

    - It's not very useful

2. Inner Join : Returns only the rows where there is a match between the specified columns in both the left (or first) and right (or second) tables.
eg., `SELECT * FROM customers c INNER JOIN orders o ON c.cust_id=o.cust_id;` // very useful query in this context! (ie, this database!)

    - In inner join, only the matching rows will be displayed!
    - For example, in the above table, some customer may never have ordered something, those will not be in the inner join table.

◉ We can also use GROUP BY with INNER JOIN : 
```psql
SELECT name FROM customers
        INNER JOIN orders
        ON orders.cust_id=customers.cust_id
GROUP BY name;

SELECT cust_name, COUNT(o.ord_id) FROM customers c
INNER JOIN 
orders o
ON o.cust_id=c.cust_id
GROUP BY cust_name;

SELECT cust_name, SUM(o.price) FROM customers c
INNER JOIN
orders o
ON o.cust_id=c.cust_id
GROUP BY cust_name;

◆ Query for finding customers name who have spend the most money in decreasing order :
SELECT c.cust_name, SUM(o.price) AS price FROM customers c INNER JOIN orders o ON o.cust_id=c.cust_id GROUP BY cust_name ORDER BY price DESC;

````

3. Left Join : 

Def : Returns all rows from the left (or first) table and the matching rows from the right (or second) table.

eg., `SELECT  * FROM customers c LEFT JOIN orders o ON c.cust_id=o.cust_id;`

    - Here, order matters! (ie, What you write on the Left side of LEFT JOIN matter!) 
    - Same is true for RIGHT JOIN case;

4. Right Join : 

Def : Returns only the matching rows from the left table and all rows from the right table.

eg., `SELECT * FROM customers c RIGHT JOIN orders o ON c.cust_id=o.cust_id;`

    - In both the case, the order in which you right the comman column doesn't matter! 
    - that is , c.cust_id=o.cust_id or o.cust_id=c.cust_id is the same, not different!

ONE TO ONE IS COMPLETED! ✅ 

### Many - Many Relationship :
Let's Understand a Use-Case of Many to Many Relationship.
Simple Example : Students And Courses (A student can take many courses and a course can have many students)

For this type of relation, a separate table is required!
```psql
◆ students : { id, student_name}
◆ student_course : { student_id, course_id } // or we can say enrollment
◆ courses : { id, course_name, fees }
```

For this, we'll make another database : institute
◆ -- is used as comments in pgadmin!
◆ JOIN can also be used instead of INNER JOIN


#### Interesting query that combines three tables!

```psql
SELECT s.name, c.name, e.enrollment_date, c.fee FROM 
enrollment e
JOIN students s ON e.s_id=s.s_id
JOIN courses c ON c.c_id=e.c_id;

-- interesting query! I need to understand it!
```

◆ We can apply aggregate functions on fee, group by on date etc!


### Tasks on Relationship

```txt
Create a one-to-many and many-to-many relationship in a shopping store context using four tables : 
        ● customers
        ● orders
        ● products 
        ● order_items
Include a price column in the products table and display the relationship between customers and thier orders, along with the details of the products in each order.

◆ customers : { cust_id, cust_name }
◆ orders : { ord_id, ord_date, cust_id }
◆ products : { p_id, p_name, price }
◆ ord_items : { items_id, ord_id, p_id, quantity }

```

![photo](https://github.com/Kishor-bharti/MD-Resources/blob/main/end%20result.png?raw=true)

◆ PDF: [pdf](https://github.com/Kishor-bharti/MD-Resources/blob/main/Postgres%20Sample%20data.pdf)


```psql
-- to solve such large problems, you need to have a diagram like the one he has in the video! That's helpful!
-- identify the most important data -> billing items ,i.e., order_items

SELECT * FROM order_items;

-- now see the resulting table, we won't p_name in place of p_id! -> for this we'll be required to connect this table with product table!
-- by ord_id, we can get cust_id, cust_name, and date of order as well!

SELECT 
                c.cust_name, 
                o.ord_date,
                p.p_name, 
                p.price,
                oi.quantity,
                (oi.quantity*p.price) AS total_price
FROM order_items oi
    JOIN
            products p ON oi.p_id=p.p_id
    JOIN
            orders o ON o.ord_id=oi.ord_id          -- now at this point our orders table is connect so we can access the customers table as well!
    JOIN 
            customers c ON o.cust_id=c.cust_id;


```

 cust_name |  ord_date  |  p_name  |  price   | quantity | total_price 
----------|------------|----------|----------|----------|-------------
 Raju      | 2024-01-01 | Laptop   | 55000.00 |        1 |    55000.00
 Raju      | 2024-01-01 | Cable    |   250.00 |        2 |      500.00
 Sham      | 2024-02-01 | Laptop   | 55000.00 |        1 |    55000.00
 Paul      | 2024-03-01 | Mouse    |      500 |        1 |         500
 Paul      | 2024-03-01 | Cable    |   250.00 |        5 |     1250.00
 Sham      | 2024-04-04 | Keyboard |   800.00 |        1 |      800.00



 ## VIEWS : 
 A view is a temporary table which we can use any time with a simple click! (like to the above table)

 ```psql
-- eg., 

CREATE VIEW billing_info AS
SELECT 
                c.cust_name, 
                o.ord_date,
                p.p_name, 
                p.price,
                oi.quantity,
                (oi.quantity*p.price) AS total_price
FROM order_items oi
    JOIN
            products p ON oi.p_id=p.p_id
    JOIN
            orders o ON o.ord_id=oi.ord_id 
    JOIN 
            customers c ON o.cust_id=c.cust_id;

-- now, after successfully creating a view, we can delete the above query and use this view instead each time we need it!

SELECT * FROM billing_info;

-- now this isn't a normal table! Infact!, if you do `\dt` you won't find it there!
-- instead we use \dv
-- so, it's basically a temporary table or a dataset
-- we have stored the query here! NOT THE DATA!!
-- changes in the tables will be reflected in the view!

 ```


 ### Having Clause GROUP ROLLUP

Having Clause : 

```psql
SELECT p_name, SUM(total_price) FROM billing_info
        GROUP BY p_name
        HAVING SUM(total_price) > 1500;
```

GROUP BY ROLLUP : 

```psql
SELECT 
            COALESCE (p_name, 'Total'),         -- Coalesce means if p_name is NULL, make it Total
            SUM(total_price) AS Amount
FROM billing_info
            GROUP BY
            ROLLUP(p_name) ORDER BY amount;
```

 coalesce |  amount   
----------|-----------
 Mouse    |       500
 Keyboard |    800.00
 Cable    |   1750.00
 Laptop   | 110000.00
 Total    | 113050.00


 ## Understanding Stored Procedure (advance topics)

 ### Stored Routine :
An SQL statement or a set of SQL Statement that can be stored on database server which can be call no. of times.

Types of STORED Routine: 
● STORED Procedure
● User defined Functions

1. STORED Procedure : Set of SQL statements and procedural logic that can perform operations such as inserting, updating, deleting, and querying data.

![stored_procedure](https://github.com/Kishor-bharti/MD-Resources/blob/main/stored%20procedure.png?raw=true)

```psql
-- eg. of a real life procedure
-- for this example, we'll use the employee table in bank_db

SELECT * FROM employees;

UPDATE employees
        SET salary = 97000
WHERE emp_id = 4;

-- now suppose that we have to run this above command for hundreds of thousands of times!
-- to save us from this, we create stored procedures! 
-- ✅ For doing repetative tasks easily, we use stored procedures!
-- its like a programing language!

CREATE OR REPLACE PROCEDURE update_emp_salary(
        p_employee_id INT,
        p_new_salary NUMERIC
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE employees
    SET salary=p_new_salary
    WHERE emp_id=p_employee_id;
END;
$$;

-- use `\df` to check stored procedures in a db
```
◆ Result 
![result](https://github.com/Kishor-bharti/MD-Resources/blob/main/result.png?raw=true)

```psql
-- ✅ Task : Make it for inserting a new data!

CREATE OR REPLACE PROCEDURE add_employee(
    p_fname VARCHAR,
    p_lname VARCHAR,
    p_email VARCHAR,
    p_dept VARCHAR,
    p_salary NUMERIC
)
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO employees (fname, lname, email, dept, salary)
    VALUES (p_fname, p_lname, p_email, p_dept, p_salary);
END;
$$;

```


2. User defined Functions | Stored Routine : custom function created by the user to perform specific operations and return a value.

◆ syntax
![stored_routine](https://github.com/Kishor-bharti/MD-Resources/blob/main/stored_routine.png?raw=true)

```psql

-- ✅ Find name of the employees in each department having maximum salary. (use case)

SELECT * FROM employees;

SELECT dept, MAX(salary) FROM 
employees
        GROUP BY dept;

-- But, we want the records of all the employees from each departments having the maximum salary!
-- for this we can use sub-queries (complex queries)

SELECT 
                e.emp_id,
                e.fname,
                e.salary
        FROM
                employees e
        WHERE e.dept = 'HR'
                AND e.salary = (
                        SELECT MAX(emp.salary)
                        FROM employees emp
                        WHERE emp.dept = 'HR'
                );

-- now, for this purpose we can create a sub routine instead of writing this huge query each time!
-- we'll now use the user defined function

-- ✅ here we go!

CREATE OR REPLACE FUNCTION dept_max_sal_emp(dept_name VARCHAR)
RETURNS TABLE(emp_id INT, fname VARCHAR, salary NUMERIC)
AS $$
BEGIN
    RETURN QUERY
    SELECT 
        e.emp_id, e.fname, e.salary
    FROM
        employees e
    WHERE
        e.dept = dept_name
        AND e.salary =  (
            SELECT MAX(emp.salary)
            FROM employees emp
            WHERE emp.dept = dept_name
        );
END;
$$ LANGUAGE plpgsql;

SELECT * FROM dept_max_sal_emp('HR');

 emp_id | fname  |  salary  
--------|--------|----------
      5 | Kavita | 47000.00

-- ✅ It's a one time process!

```

## What are Windows Function 

1. Window Function : Window functions, also known as analytic functions allow you to perform calculations across a set of rows related to the current row.

Defined by an OVER() clause.




