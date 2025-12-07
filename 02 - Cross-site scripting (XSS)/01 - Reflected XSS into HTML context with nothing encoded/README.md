# Lab: Reflected XSS into HTML context with nothing encoded

This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.

To solve the lab, perform a cross-site scripting attack that calls the `alert` function.


---------------------------------------------

Source : https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded

---------------------------------------------

## Brief Analysis
This lab demonstrates a Reflected XSS vulnerability that occurs because input from the search parameter is reflected back to the HTML page without any encoding or sanitization. When a user fills out the search form, the entered value will reappear in the element:

```sh
<h1>0 search results for 'USER_INPUT'</h1>
```

Since there is no security mechanism such as HTML escaping, special characters and HTML tags can be inserted directly into the page. This allows attackers to insert `<script>` and make the browser execute malicious JavaScript. Thus, a simple injection such as the one below is executed immediately and proves that the application is vulnerable to Reflected XSS in HTML context (nothing encoded):

```sh
<script>alert(1)</script>
```

## Solution Steps
#### 1. Click the search input form
Enter a normal input to see the response. Here, I entered the word “hello” and you can see that the result of the input was returned by the server and displayed again on the web page.
<img width="950" height="397" alt="Image" src="https://github.com/user-attachments/assets/381a941e-a1f7-4adb-8aea-fc6964404587" />

#### 2. Check “View source page”
From the page source below, it can be seen that the results of searching for the word “hallo” do not undergo security mechanisms such as HTML escaping and are executed immediately.
<img width="769" height="358" alt="Image" src="https://github.com/user-attachments/assets/891b5094-7ae7-454d-a0b4-5644f9ae56bc" />

#### 3. Enter a simple XSS payload
Next, insert a simple XSS payload into the search input form.

```sh
<script>alert(1)</script>
```
<img width="925" height="323" alt="Image" src="https://github.com/user-attachments/assets/a0aa52c1-69d7-4f20-a9cd-26c3501a6df4" />

#### 4. Check the results
When the xss payload is sent, the web page will display a popup alert indicating that the attack was successful.
If there are no errors, the result will be as shown below.
<img width="580" height="241" alt="Image" src="https://github.com/user-attachments/assets/a22c74c5-0019-441b-b96b-a74ef5c3c35e" />
<img width="1444" height="604" alt="Image" src="https://github.com/user-attachments/assets/287ce3a5-f9cc-4760-b882-e1017798dafd" />
