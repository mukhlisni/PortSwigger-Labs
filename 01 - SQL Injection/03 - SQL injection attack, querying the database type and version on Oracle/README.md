# Lab: SQL injection attack, querying the database type and version on Oracle

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

---------------------------------------------

Source : https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle

---------------------------------------------

## Brief Analysis

This lab has an SQL injection vulnerability in the Category parameter, where user input is directly embedded into the query without being sanitized. This allows attackers to insert additional SQL commands.

```sh
/filter?category=Gifts
```

## Solution Steps
#### 1. Find out the type of database
From the lab display, it is clear that the type of database used is Oracle.
<img width="1450" height="244" alt="Image" src="https://github.com/user-attachments/assets/1a64a23b-7bd7-472d-8a1a-09396cd15b11" />

#### 2. Select one of the Categories
#### 3. Determining the number of columns with Burp Suite
The website and HTML display two columns (title and description), but it is necessary to confirm the number of columns in the SQL query.
<img width="1458" height="627" alt="Image" src="https://github.com/user-attachments/assets/066a90fb-aadb-4325-9525-4aadd1dea9c7" />

To determine the number of columns, use this payload:
```sh
' ORDER BY <number>--
```

When using numbers 1 and 2, the server responds with 200.
`'+ORDER+BY+1--` and `'+ORDER+BY+2--`
<img width="1490" height="448" alt="Image" src="https://github.com/user-attachments/assets/a3dfb0a1-511a-4a18-9cd0-7042825a93be" />

<img width="1490" height="405" alt="Image" src="https://github.com/user-attachments/assets/a90bec31-305f-4c8d-8f84-237efc6c7e58" />

When using the number 3, the response given is 500 with the description Interval Server Error.
<img width="1491" height="409" alt="Image" src="https://github.com/user-attachments/assets/55f9d0ef-f820-435a-bd69-476ddfb4b40b" />

From this information, it can be confirmed that there are two columns.
Next, check whether the input entered will be displayed on the website with a command like this:
```sh
'+UNION+SELECT+'asd','asd'+FROM+DUAL--
```
Since we do not know the name of the table to be used, we will use a dummy table (DUAL). DUAL is a dummy table (special table) used in Oracle Database to perform SELECT without having to read from the actual table.

The result is as follows:
<img width="1456" height="353" alt="Image" src="https://github.com/user-attachments/assets/fce8cb96-b71c-40a2-ae0f-e6b437447c87" />

#### 4. Displaying the database version
Since we already know that the database type used is Oracle, the command used to display the database version is 
```sh
SELECT banner FROM v$version. 
```

The complete command used is 
```sh
'+UNION+SELECT+banner,+NULL+FROM+v$version--
``` 
banner is the name of a column (field) in an Oracle table/dynamic view named v$version.

#### 5. Check the results
If the command runs correctly, the website will display its database version information.
<img width="1319" height="865" alt="Image" src="https://github.com/user-attachments/assets/56de8626-79a1-4714-b7c6-36b9a5990c26" />

---------------------------------------------

Reference : https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------
