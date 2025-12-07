# Lab: Stored XSS into HTML context with nothing encoded

This lab contains a stored cross-site scripting vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the alert function when the blog post is viewed.


---------------------------------------------

Source : https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded

---------------------------------------------

## Brief Analysis
This lab contains a Stored Cross-Site Scripting (XSS) vulnerability in the blog comment feature. When a user submits a comment, the input is stored on the server and will be displayed again on the blog post page. This occurs because user input is not filtered or sanitized, and comments are displayed again in HTML context without encoding.

```sh
<p>[user_comment]</p>
```

If the input contains a `<script>` tag, the browser will treat it as part of the HTML document and execute the JavaScript within it.

The vulnerability is persistent (stored), which means that the malicious payload will remain stored in the database and will be executed every time the page is accessed by anyone, including admins or other users.

## Solution Steps
#### 1. Click on one of the posts
Scroll down and you will see an input form for comments.

#### 2. Enter a regular comment
To check for stored XSS vulnerabilities, enter a normal comment into the input box as shown in the image below.
<img width="906" height="761" alt="Image" src="https://github.com/user-attachments/assets/a3a4529a-bb31-408a-b0a7-941fcf83b3b3" />

If after sending a comment the display looks like this, click “Back to blog” to return to the previous post.
<img width="897" height="237" alt="Image" src="https://github.com/user-attachments/assets/7b3e62e9-574f-4c04-ae61-c95eaf0a65ad" />

Then, look at the comments section to see that the comment you just entered is now in the comments column.
<img width="917" height="651" alt="Image" src="https://github.com/user-attachments/assets/948dcbd6-16fe-4eae-bb04-5ea2c7fc8aaf" />

#### 3. Check “View source page”
From the image below, it can be concluded that the input of the word “hello” was not sanitized.
<img width="1549" height="699" alt="Image" src="https://github.com/user-attachments/assets/4a321468-7991-4544-b8aa-69cca1a257c6" />

#### 4. Send a simple XSS payload
Now send a simple XSS payload in the comment form and check the results.
```sh
<script>alert(1)</script>
```
<img width="911" height="740" alt="Image" src="https://github.com/user-attachments/assets/ae7ded19-dbed-4a67-a878-9c454b9ed0d1" />

#### 5. Check the results
If the payload is correct and there are no errors, the result will be as shown in the image below.
<img width="582" height="234" alt="Image" src="https://github.com/user-attachments/assets/9c0a886b-ab63-439d-9f26-45a4ca7fb280" />
<img width="1433" height="306" alt="Image" src="https://github.com/user-attachments/assets/6389392b-4761-4f73-b6d9-3feee1660c2f" />
