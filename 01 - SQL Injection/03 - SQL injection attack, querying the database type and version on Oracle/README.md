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
#### 3. Determining the number of columns
The website and HTML display two columns (title and description), but it is necessary to confirm the number of columns in the SQL query.
![alt text](image.png)

#### 4. Check the results
If the login is successful, the display will look like this and you will now be logged in as an administrator.

