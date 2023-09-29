# SQL

## mySql / MariaDB

<https://dev.mysql.com/doc/refman/8.0/en/keywords.html> - Reserved words  
Backquote to quote, although single quote works too.

Create DB

```SQL
CREATE SCHEMA IF NOT EXISTS `myschema` DEFAULT CHARACTER SET utf8 ;
CREATE DATABASE mydb CHARACTER SET utf8 COLLATE utf8_general_ci;
USE databasename;
```

Overview, list all tables, cols

```SQL
SELECT VERSION(), DATABASE(), CONNECTION_ID(), USER(), CURRENT_USER();
SELECT TABLE_NAME FROM information_schema.TABLES WHERE TABLE_SCHEMA = 'mydb';
SELECT * FROM information_schema.`COLUMNS` WHERE TABLE_NAME = 'mytable';
SELECT * FROM mysql.user;
```

Typical Create

```SQL
DROP TABLE IF EXISTS my_table;
CREATE TABLE my_table (
  id int IDENTITY(1,1) NOT NULL,
  foreign_table_id int DEFAULT NULL,
  seq int DEFAULT NULL,
  is_active int NOT NULL DEFAULT '1',
  activated_dt DATETIME DEFAULT NULL,
  description VARCHAR(2000) DEFAULT NULL,
  created_user varchar(20) NOT NULL,
  created_dt datetime NOT NULL DEFAULT CURRENT_TIMESTAMP(),
  lastedit_user varchar(20) DEFAULT NULL,
  lastedit_dt datetime DEFAULT NULL,
  PRIMARY KEY (id),
  KEY seq (seq))
ENGINE = InnoDB
AUTO_INCREMENT = 1
DEFAULT CHARACTER SET = utf8;
```

Engine could also be MyISAM

```SQL
ALTER TABLE my_table ADD new_col_name VARCHAR(50) NULL;
```

How to drop all tables. Builds a script, doesn't auto-run

```SQL
USE information_schema;
SELECT CONCAT('DROP TABLE ',table_name,';') fld FROM TABLES WHERE table_schema = '<this_db_name>';
```

Find skipped id numbers from a table "t" mytable

```SQL
SELECT * FROM (
SELECT @id1+1 first_empty_id, id-@id1-1 count_of_empty_ids, @id1 := id this_id
FROM mytable t, (SELECT @id1 := 1) dummy_table
ORDER BY id ) t1
WHERE t1.count_of_empty_ids > 0;
```

Date/Time

* Don't use `SYSDATE()`, use `NOW()`. Problem: `SYSDATE()` will execute upon each use, `NOW()` is single execute.
* `SELECT YEAR(SYSDATE())`
* `SELECT SYSDATE()` - this is the at-the-moment system date/time, can fire n times in a single query
* `SELECT DATETIME()` - Same as sysdate but only fires once for the query. Get the same date/time for multiple inserts or rows.
* `SELECT CURTIME()`
* `SELECT DATEDIFF(DATE(FLOOR(SYSDATE())),'2000-01-01');` - days since a date

Other cool stuff

* `SELECT LEAST(NULL,500);`
* `SELECT LAST_INSERT_ID();` - most recent inserted ID from an auto increment

-- # of rows that would have been returned if there was no "limit". Follow-up query.
SELECT user_id
FROM site_user
LIMIT 10;
SELECT FOUND_ROWS();

<https://cheatography.com/davechild/cheat-sheets/mysql/> - mostly for type definitions and fn names

*`TODO` adding indexes, other data types (blobs in particular), foreign constraints, full text search example or reference. Plus I have a snipit cache somewhere, add it*

## Microsoft SQL Server

* <https://cheatography.com/davechild/cheat-sheets/sql-server/> - terrible cheatsheet, but a start

* [Reserved Words](https://learn.microsoft.com/en-us/sql/t-sql/language-elements/reserved-keywords-transact-sql?redirectedfrom=MSDN&view=sql-server-ver16) - Important resources. Includes ODBC reserved words and earmarked future reserved (LOTS of future gotchas).

*`TODO` scripted conversion from MySql code to add*

## Oracle

[Reserved words](https://docs.oracle.com/en/database/oracle/oracle-database/21/zzpre/Oracle-reserved-words-keywords-namespaces.html) - Includes PL/SQL

Converting Oracle into mySql: "Create Table" statements (specific need)

* Zap any database name e.g., database_name.table_name --> table_name
* replace "prompt" with "--"
* For primary key, often replace NUMBER with AUTO_INCREMENT
* `NUMBER` --> `INT`
  * Watch out for embedded "NUMBER" text in fields.
  * Some like NUMBER(1) need to be TINYINT(1)
* `VARCHAR2` --> `VARCHAR`
* `DATE DEFAULT SYSDATE` --> `TIMESTAMP DEFAULT CURRENT_TIMESTAMP`
* Add `PRIMARY_KEY(<field_name>)`
* append `ENGINE=INNODB DEFAULT CHARSET=utf8`

Converting Oracle insert statements to mySql (also very specific need)

* Watch out for reserved words. e.g., change " name, " to " `name`, "
* Zap the date inserts.
  * Search Mask, w/ unix regex: `to_date('\d{2}-\d{2}-\d{4} \d{2}:\d{2}:\d{2}', 'dd-mm-yyyy hh24:mi:ss')`
  * Set it to null or now().

Cool Oracle Stuff

* If you add a comment into your select statement of "/*csv*/" and run as a script (important!) then it will output CSV strings!
  * `select /*csv*/ field1, field2 from table;`
* To insert a single quote, just quote the quote.
  * 'The school''s name is ''My High School'' and that''s it.'
* To insert a "&", it can be escaped with a "\". However I've has some trouble with this. Only a problem in sqlPlus or sqlDeveloper.
  * To turn off the feature of using "&" for variables, run `SET DEF OFF;` then run the insert or update query.

SELECT @row := @row + 1 AS ROW, t.*
FROM site_user t, (SELECT @row := 0) r

```SQL
grant select on SCHEMA.TABLE to SOMEUSER;
```

## SQLite

* <https://sqlite.org/cli.html> - CLI
  * `sqlite3 <databasename>`
* most used (other than queries): `.tables`, `.schema ?<table>?` or `.fullschema`, `.dump <table>`
* `.header`, then `.excel`, then execute the query.
