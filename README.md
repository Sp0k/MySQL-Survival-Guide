# MySQL Survial Guide
This is my personal MySQL help file. It is catered for Ubuntu.

It was written for Dalhousie's CSCI 2141: Intro to Databases.

Last Updated: 2024-01-31<br>
Up-to: Lecture 3

@author: Gab Savard<br>
Based on Dr. Beiko and Dr. Malloch's work

## Utilities for MySQL
### Start the MySQL dbs
sudo systemctl start mysql<br>or<br>startMySQL (Personal script)

### Stop the MySQL dbs
sudo sustemctl stop mysql<br>or<br>stopMySQL (Personal script)

### Connect to the server
sudo mysql -u root<br>or<br>connectMySQL (Personal script)

### Read a file into the server
sudo mysql -u root < [filename]<br>or<br>readMySQL [filename] (Personal script)

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

