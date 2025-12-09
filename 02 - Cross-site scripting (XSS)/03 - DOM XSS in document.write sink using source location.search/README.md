# Lab: DOM XSS in `document.write` sink using source `location.search`

This lab contains a DOM-based cross-site scripting vulnerability in the search query tracking functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search, which you can control using the website URL.

To solve this lab, perform a cross-site scripting attack that calls the alert function.


---------------------------------------------

Source : https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink

---------------------------------------------

## Brief Analysis
This challenge demonstrates the vulnerability of DOM-Based Cross-Site Scripting (DOM XSS), which occurs entirely on the client side. This happens because the data is manipulated directly by JavaScript in the browser.

The vulnerability originates from the following code:
```sh
function trackSearch(query) {
    document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
}
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
    trackSearch(query);
}
```

The application is vulnerable because it takes input from the URL and then processes it directly using `document.write()` without encoding. This allows attackers to break the HTML structure and insert new elements that trigger JavaScript execution. As a result, DOM XSS occurs and can be exploited simply by manipulating URL parameters, even without server interaction.

## Solution Steps
#### 1. Click on the search input form
Enter random text to see how the application works.

<img width="950" height="360" alt="Image" src="https://github.com/user-attachments/assets/3e43f5c9-1199-41ef-939e-1663bb58e09b" />

Then, right-click and select “View source page” or CTRL+U to see the results of the search.

<img width="839" height="392" alt="Image" src="https://github.com/user-attachments/assets/ad2ab17f-3dfc-4126-aeda-1ba8323de2ae" />

You can see that the query is taken directly from location.search, which can be influenced by the user. The value is written directly to HTML via document.write, which means there is a dangerous XSS sink, as well as no validation or sanitization.

Since the `query` value is entered into `img src`.
```sh
<img src="/resources/images/tracker.gif?searchTerms=USER_INPUT">
```
Therefore, to exploit XSS, we must close the src attribute using `">`

#### 2. Injecting DOM XSS payloads
The payload used to insert new elements and execute JavaScript is as follows:
```sh
"><svg onload=alert(1)>
```
This payload works because the `">` character closes the `<img>` and `<svg>` tags, turning them into new valid tags. After that, the `onload` attribute will execute JavaScript when the tags are processed by the browser.

<img width="907" height="283" alt="Image" src="https://github.com/user-attachments/assets/6664b3bd-8cf7-43b3-a5db-1ac2c1aec8cf" />

#### 3. Check the results
If there are no errors in typing the payload, the result will be as shown in the image below. 

<img width="567" height="225" alt="Image" src="https://github.com/user-attachments/assets/2909db1a-c7c8-4029-bce7-8372a3995591" />

