# MySQL Survial Guide
This is my personal MySQL help file. It is catered for Ubuntu.

It was written for Dalhousie's CSCI 2141: Intro to Databases.

Last Updated: 2024-02-08<br>
Up-to: Lecture 3

@author: Gab Savard<br>
Based on Dr. Beiko and Dr. Malloch's work

## Utilities for MySQL
#### Start the MySQL dbs
```bash
sudo systemctl start mysql
```
or<br>startMySQL (Personal script)

#### Stop the MySQL dbs
```bash
sudo sustemctl stop mysql
```
or<br>stopMySQL (Personal script)

#### Connect to the server
```bash
sudo mysql -u root
```
or
```
mysql -u 'username' -p
```
Where <em>-p</em> is only added if the user has a password set. A
prompt will appear asking for the password.

or<br>connectMySQL (Personal script)

#### Read a file into the server
```bash
sudo mysql -u root < [filename]
```
or
```
mysql -u root -p < [filename]
```
or<br>readMySQL [filename] (Personal script)


** <em>All of these can also be done through workbench</em> **

## Inside MySQL
### Navigation
You will need specific commands to traverse the dbs. They are listed below

#### SHOW
The equivalent of 'ls' in bash.
```mysql
-- Shows all elements of the type precised
SHOW DATABASES
SHOW TABLES
```
#### USE
Opens the table or database requested
```mysql
USE DatabaseDB
```
* Usually, databases name will be capitalized and have a DB at the end i.e. DatabaseDB. Table names will be lowercase.

### Queries
#### SELECT
The SELECT query generates customized results. It does NOT modify the data in the table!

```mysql
SELECT [columns]
  FROM [table]
  WHERE [criteria]
  ORDER BY [column_name]
  LIMIT [max_rows];

SELECT * -- retrive and display every columns from a table. Still requires a FROM attribute.
```
#### AS
Gives a display name to a column when printing the results of the query.
```mysql
SELECT [columns] AS 'name'
  [attribute] AS 'name1', [attribute] AS 'name2'
  FROM [table];
```
#### WHERE
Specifies one or more criteria that restricts the rows returned.<br>
- Comparison operator: =, >, <, <=, >=, <>, !=, BETWEEN, LIKE
- Logical operator: AND, OR, NOT
- Set operator: IN
- Defined: IS [NOT] NULL

* Matching is NOT case sensitive

##### Comparison
These operators mean the same thing as what they would mean in different coding languages. There is however specific ones to MySQL.
- = means equal to
- \> means bigger than, < means smaller than
- \>= and <= respectively mean bigger or equal than and smaller or equal than
- <> and != both mean not equal to
- BETWEEN declares a range for the value to be between
- LIKE adds more flexibility to string matching (There will be more information about it lower)

Example:
```mysql
SELECT [columns]
  FROM [table]
  WHERE [attribute] = 'value'; -- This case looks for the elements that match the value.
```
##### Logical
Allow more than one condition for the different attributes.
- AND: Equivalent to && in C, the condition is true only if all conditions are respected.
- OR: Equivalent to || in C, the condition is true if at least one of the conditions are respected.
- NOT: Equivalent to ! in C, the conditions is true if it is not respected

Example:
```mysql
SELECT [columns]
  FROM [table]
  WHERE ([attribute_1] = 'value_1' OR [attribute_1] = 'value_2') AND NOT attribute_2 = 'value_3';
```
##### Set
The set operator IN (the only one seen in this guide) is a shorthand for multiple OR operator for the same attribute.

Example:
```mysql
SELECT [columns]
  FROM [table]
  WHERE [column] IN (value_1, value_2, ...);
```
##### LIKE
Like mentioned above, the LIKE operator adds more flexibility to string matching. It is still limited though, it can't easily match:
- Any string that has the same first and last letter ("Australia", "Asia")
- Specific character classes ("F" followed by a vowel requires 5 OR statements)
- Guaranteed case matching
- Return subsets of strings
- Requires regular expressions for this (Out of scope for this guide)

The LIKE operator uses two special characters to allow wildcard matches:<br>
%: match zero or more characters of any type<br>
_: match exactly one character of any type

May or May note be case sensitive.

Examples:<br>
LIKE with '%'
```mysql
-- Looks for everything that starts with "A" or "a"
SELECT [columns]
  FROM [table]
  WHERE [attribute] LIKE "%A";

-- Looks for everything that does not contain "A" or "a"
SELECT [columns]
  FROM [table]
  WHERE [attribute] NOT LIKE "%A%";
```
LIKE wildcard matching
```mysql
-- Looks for everything containing 'rain'
SELECT [columns]
  FROM [tables]
  WHERE [attribute] LIKE '%rain%';

-- Looks for everything starting by a character followed by 'rain'
SELECT [columns]
  FROM [table]
  WHERE [attribute] LIKE '_rain';
-- Each _ added means one extra character.
```
Multiple LIKE conditions
```mysql
-- Looks for every string where at least one of them starts with 'A'
SELECT [columns]
  FROM [table]
  WHERE [attribute] LIKE "A% %" -- First word
  OR [attribute] LIKE "% A%"; -- Non-first word
```
NOT LIKE
```mysql
-- Looks for every string who's second character is not "A"
SELECT [columns]
  FROM [table]
  WHERE [attribute] NOT LIKE "_A%";
```
#### CREATE
The operations can be used directly in the MySQL server or written on a file along with the INSERT operations and read and ran into the server later.

##### DATABASE
There is two options to create new databases.<br>
Option 1: Don't create a new one on top of an old one.
```mysql
-- Ensures if the database already exist it won't be overwrittten
CREATE DATABASE IF NOT EXISTS [NameDB];
```
Option 2: Ensure a new one is created (Will cause an error if the database does not already exist)
```mysql
-- Erases the database and recreates it
DROP DATABASE [NameDB];
CREATE DATABASE [NameDB];
```

* A database name usually has a first capitalized letter and ends with a capitalized DB, i.e. CitiesDB

Both option of database creation can also be used together to ensure less error.
```mysql
DROP DATABASE IF EXISTS [NameDB];
CREATE DATABASE IF NOT EXISTS [NameDB];
```
##### TABLE
There is also two options to create new tables. The same logic can also be used to combine both options.<br>
Option 1: Don't create a table with the same name as an existing one
```mysql
CREATE TABLE IF NOT EXISTS [table_name] (...);
```
Option 2: Ensure a new fresh table is created
```mysql
DROP TABLE [table_name];
CREATE TABLE [table_name];
```
When creating a new table, one should add parameters to the table
```mysql
CREATE TABLE [table_name] (
  column_1 DATATYPE_CONSTRAINT(S),
  column_2 DATATYPE_CONSTRAINT(S),
  PRIMARY KEY (column_1,...)
);
```
Example of a table creation taken from Dr. Beiko's slides:
```mysql
CREATE TABLE cities_simplified (
  city_id INT,
  city_name VARCHAR(100) NOT NULL,
  country_name VARCHAR(100) NOT NULL,
  year_founded INT,
  PRIMARY KEY (city_id),
  UNIQUE(country_name),
  UNIQUE(city_name, year_founded)
);
```
##### DATATYPES
<strong>Character datatypes:</strong>
- VARCHAR(n): Variable-length character string (n <= 65535)
- TEXT: Unstructured text. Can be much longer, but very slow.

<strong>Numeric datatypes:</strong>
- INT: Integer (signed by default), signed range is (-2147483647, 2147483647)
- FLOAT, DOUBLE: 32-bit/64-bit floating point
- DECIMAL(x,y): x represent the total digits, y represent the number of digits after the decimal point. (ex: DECIMAL(314, 2) = 3.14).

##### CONSTRAINTS
Define limitations on columns. You cannot insert data into a table
that violates those constraints.

- NOT NULL: Column must be defined for every entity that is added.
- UNIQUE: Duplicate values cannot be added.
- DEFAULT: If no value is specified, set the value to this.
- CHECK: Allow entity to be added only if column of table values
satisfy constraints.

<strong>Examples:</strong>
These examples are taken directly from Dr. Beiko's slides.

Create table using DEFAULT:
```mysql
CREATE TABLE cities_simplified(
  city_id INT,
  city_name VARCHAR(100) NOT NULL,
  country_name VARCHAR(100) NOT NULL,
  year_founded INT DEFAULT 2024,
  PRIMARY KEY(city_id),
  UNIQUE(country_name),
  UNIQUE(city_name, year_founded)
);
```
Create table using CHECK constraints:
```mysql
CREATE TABLE cities_simplified(
  city_id INT,
  city_name VARCHAR(100) NOT NULL,
  country_name VARCHAR NOT NULL,
  year_founded INT
    CONSTRAINTS 'year_range'
    CHECK(year_founded BETWEEN -5000 and 2024),
  PRIMARY KEY(city_id),
  UNIQUE(country_name),
  UNIQUE(city_name, year_founded)
);
```
Create table with AUTO_INCREMENT:
```mysql
CREATE TABLE cities_simplified(
  city_id INT AUTO_INCREMENT,
  city_name VARCHAR(100) NOT NULL,
  country_name VARCHAR(100) NOT NULL,
  year_founded INT,
  PRIMARY KEY(city_id),
  UNIQUE(country_name),
  UNIQUE(city_name, year_founded)
);
```
#### ALTER
The ALTER query can be used to modified the data already saved in the
tables.

Be careful, sometimes, modification will be rejected if it contradicts
the data already in the database.

<strong>Table Examples:</strong><br>
Add a column:
```mysql
ALTER TABLE table_name
  ADD (new_colname DATATYPE constraints);
```
Drop a column:
```mysql
ALTER TABLE table_name 
  DROP COLUMN column_name;
```
Modify column type:
```mysql
ALTER TABLE table_name 
  MODIFY COLUMN column_name new_datatype;
```
Change column name:
```mysql
ALTER TABLE table_name 
  CHANGE old_column_name new_column_name;
```
Add primary key:
```mysql
ALTER TABLE table_name 
  ADD PRIMARY KEY (column_name);
```
Drop primary key:
```mysql
ALTER TABLE table_name 
  DROP PRIMARY KEY;
```
Add foreign key:
```mysql
ALTER TABLE table_name 
  ADD CONSTRAINT fk_name 
  FOREIGN KEY (column_name) 
  REFERENCES parent_table(parent_column_name);
```
Drop foreign key:
```mysql
ALTER TABLE table_name 
  DROP FOREIGN KEY fk_name;
```
Add index:
```mysql
ALTER TABLE table_name 
  ADD INDEX index_name (column_name);
```
Drop index:
```mysql
ALTER TABLE table_name 
  DROP INDEX index_name;
```
Rename table:
```mysql
ALTER TABLE old_table_name 
  RENAME TO new_table_name;
```
Add unique constraint:
```mysql
ALTER TABLE table_name 
  ADD UNIQUE (column_name);
```
Drop unique constraint:
```mysql
ALTER TABLE table_name 
  DROP INDEX index_name;
```

#### DROP
Deletes the specified table or database.

Database:
```mysql
DROP DATABASE Database_nameDB;
```

Table:
```mysql
DROP TABLE table_name;
```

#### INSERT
The INSERT statement is used to add new entities(rows) to a table.
```mysql
INSERT INTO table_name(column_name1, column_name2, column_name3, ...)
VALUES (value1, value2, value3, ...);
```
or
```mysql
INSERT INTO table_name(column_name1, column_name2, column_name3, ...)
VALUES
(value1, value2, value3, ...),
(value1, value2, value3, ...),
(value1, value2, value3, ...),
...
(value1, value2, value3, ...);
```

One can also use the IGNORE keyword in order to force the entry of
the data even if it violates the constraints. IGNORE will only result
in a warning. But this must be used with precaution.

#### UPDATE
The UPDATE statement is used to update a table with new values for 
columns and entities.
```mysql
UPDATE table_name
  SET column1 = value1, column2 = value2, column3 = value3, ...
  WHERE conditions;
```

#### DELETE
The DELETE statement is used to delete a row in a table.
```mysql
DELETE FROM table_name
  WHERE conditions;
```

#### Derived Attributes
In addition to returning values contained in the table, one can combine
values from different columns using various operators.

* Mathematical operators: +, -, *, /
* Text operators: CONCAT()

The idea is to be able to use them to mix values, for example calculating the population density:
```mysql
SELECT population/land_area
  FROM table_name
  WHERE condition;
```
<strong>Rounding</strong><br>
ROUND(attribute, digits): Round attribute to digits of precision (default is zero).

```mysql
SELECT ROUND(1.1111); -- Will be equal to 1
SELECT ROUND(1.1111, 2); -- Will be equal to 1.11
```

The ROUND command can renamed with an alias using AS.
```mysql
SELECT column1, column2, column3, ROUND(column3/column2, 1) AS alias
  FROM table_name;
```
** Reminder that the WHERE clause does not recognize aliases. **
