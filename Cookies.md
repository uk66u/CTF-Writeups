# 🍪 Cookies — picoCTF 2021

## Challenge Information

| Field | Details |
|---|---|
| **Platform** | picoCTF |
| **Challenge** | Cookies |
| **Category** | Web Exploitation |
| **Difficulty** | Easy |
| **Tools Used** | Chrome DevTools, Burp Suite |
| **Technique** | Cookie Manipulation |

---

## Challenge Description

The challenge provides a web application that uses an HTTP cookie to determine which content is returned to the user.

The objective was to inspect the application's cookies, understand how the cookie value affected the server response, and identify the value that reveals the flag.

---

## Initial Analysis

After launching the challenge, I interacted with the web application and noticed that different cookie selections produced different responses.

To investigate further, I opened **Chrome Developer Tools** using:

```text
F12
```

Then I navigated to:

```text
Application → Storage → Cookies
```

I found a cookie named:

```text
name
```

The cookie contained a numeric value.

This suggested that the application was using the value of the `name` cookie to determine which content should be returned.

---

## Method 1 — Chrome DevTools

### Step 1: Inspect the Cookie

Using the **Application** tab in Chrome DevTools, I inspected the cookies associated with the challenge website.

The relevant cookie was:

```text
name
```

Its value could be modified directly from the browser.

### Step 2: Modify the Cookie Value

I changed the value of the `name` cookie and refreshed the page.

For example:

```text
name=12
```

The application responded with:

> That is a cookie! Not very special though...

This confirmed that changing the cookie value affected the server response.

### Step 3: Test Different Values

I continued testing different numeric values:

```text
13 → 14 → 15 → 16 → 17 → 18
```

Each value produced a different response.

When the value was changed to:

```text
name=18
```

the application returned the flag.

---

## Method 2 — Burp Suite Repeater

To analyze the HTTP request more directly, I repeated the process using **Burp Suite**.

### Step 1: Capture the Request

I opened the challenge through Burp Suite and captured the request sent to the:

```text
/check
```

endpoint.

The request contained the following cookie:

```http
Cookie: name=17
```

I sent the request to **Burp Repeater** so I could modify the cookie value and resend the request without manually refreshing the browser.

---

### Step 2: Test `name=17`

Inside Burp Repeater, I sent the request with:

```http
Cookie: name=17
```

The server returned a normal response:

> I love wafer cookies!

This confirmed that the request was valid, but the value `17` did not reveal the flag.

### Burp Suite — Cookie Value 17

![Burp Suite testing Cookie name=17](burp-cookie-17.png)

---

### Step 3: Change the Cookie to `name=18`

Next, I changed:

```http
Cookie: name=17
```

to:

```http
Cookie: name=18
```

Then I clicked **Send** again in Burp Repeater.

---

### Step 4: Analyze the Response

This time, the server returned a different response.

The HTML response contained:

```text
Flag
```

followed by the challenge flag.

### Burp Suite — Cookie Value 18

![Burp Suite Cookie name=18 returning the flag](burp-cookie-18-flag.png)

The correct cookie value was therefore:

```http
Cookie: name=18
```

---

## Flag

```text
picoCTF{REDACTED}
```

---

## Technical Analysis

The application relied on a client-controlled cookie value to determine which content should be returned.

The relevant request contained:

```http
Cookie: name=<value>
```

By modifying `<value>`, it was possible to influence the server response.

The important observation was:

```text
name=17 → Normal response
name=18 → Flag returned
```

Because HTTP cookies are stored and controlled on the client side, users can modify them using browser Developer Tools or an intercepting proxy such as Burp Suite.

This is why applications should never assume that cookie values received from a client are trustworthy.

---

## Security Concept

This challenge demonstrates **client-side cookie manipulation** and the security risks associated with trusting user-controlled input.

Cookies can be modified before an HTTP request reaches the server.

For security-sensitive functionality, applications should perform proper **server-side validation** and should not rely solely on client-controlled values for authorization or access-control decisions.

---

## Tools Used

### Chrome Developer Tools

Used to:

- Inspect stored cookies.
- Identify the `name` cookie.
- Modify cookie values.
- Observe changes in the application's response.

### Burp Suite Repeater

Used to:

- Inspect the raw HTTP request.
- Identify the `Cookie` request header.
- Modify the `name` cookie.
- Resend requests with different values.
- Compare server responses.
- Confirm that `name=18` returned the flag.

---

## What I Learned

This challenge helped me understand:

- How HTTP cookies are sent between a browser and a web server.
- How to inspect and modify cookies using Chrome DevTools.
- How cookies appear inside raw HTTP requests.
- How to capture and analyze HTTP requests using Burp Suite.
- How to use Burp Repeater to modify and resend requests.
- How changing client-controlled data can affect server responses.
- Why applications should not trust client-controlled values.
- Why server-side validation is important for security-sensitive operations.

---

## Conclusion

The challenge was solved by identifying the client-controlled `name` cookie and testing different numeric values.

Using Chrome DevTools showed that modifying the cookie changed the application's behavior.

Burp Suite Repeater provided a clearer view of the underlying HTTP request and confirmed that changing:

```http
Cookie: name=17
```

to:

```http
Cookie: name=18
```

caused the server to return the flag.

The main security takeaway from this challenge is:

> **Never trust client-controlled data for security-sensitive decisions. Always validate sensitive operations on the server side.**
