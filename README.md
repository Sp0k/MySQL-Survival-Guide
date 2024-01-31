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

SELECT * -- retrive and display every columns from a table.
```
#### AS
Gives a display name to a column when printing the results of the query.
```mysql
SELECT [column] AS 'name'
  [attribute] AS 'name1', [attribute] AS 'name2'
  FROM [table];
