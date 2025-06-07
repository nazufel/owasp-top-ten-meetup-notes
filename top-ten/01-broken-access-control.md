# A01:2021 - Broken Access Control

## Overview
According to [OWASP](https://owasp.org/Top10/A01_2021-Broken_Access_Control/), Broken Access Control occurs when restrictions on what authenticated users are allowed to do are not properly enforced. This can lead to unauthorized information disclosure, modification, or destruction of data, as well as performing business functions outside of the user's limits.

Access control enforces policy such that users cannot act outside of their intended permissions. Failures typically lead to unauthorized information disclosure, modification or destruction of all data, or performing a business function outside of the limits of the user.

### Reconnaissance
I was interested in the shopping basket. The url in the browser for a logged in user didn't have a basket ID. I expected there to be a `/basket/1234` in the url so that the web server knew what basket to look up. However, the path was just `/basket`. 

Running some requests through Burp, I saw that did infact know that the basket ID was. 

![Basket ID in Burp Suite](images/a01-2021-basket-id.png)

The question was, where was the browser getting that information? I couldn't find any requests to the server looking for the basket ID. That informatoin had to be stored locally on the client. Maybe in a cookie or in session storage?

Yes. It was session storage.

![Basket ID in Burp Suite](images/a01-2021-starting-basket-id.png)

### Exploit

### Impact

#### Classification

### Remediation
