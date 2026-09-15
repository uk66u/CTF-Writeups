# Cookies 🍪

## Challenge Information

- **Platform:** picoCTF
- **Category:** Web Exploitation
- **Difficulty:** Easy
- **Challenge:** Cookies

## Challenge Description

The challenge asks us to find the correct cookie value in order to retrieve the flag.

## Steps

### 1. Open the Challenge

I started the challenge and opened the provided website.

### 2. Inspect the Cookies

I opened Chrome Developer Tools by pressing:

`F12`

Then I navigated to:

`Application → Storage → Cookies`

I found a cookie named:

`name`

The cookie had a numeric value.

### 3. Modify the Cookie Value

I changed the value of the `name` cookie and refreshed the page.

For example:

`name=12`

The website responded with:

> That is a cookie! Not very special though...

This indicated that different cookie values produced different responses.

### 4. Find the Correct Value

I continued testing different values:

`13 → 14 → 15 → 16 → 17 → 18`

After changing the cookie value to:

`name=18`

and refreshing the page, the flag was displayed.

## Flag

`picoCTF{REDACTED}`

## What I Learned

This challenge helped me understand:

- How HTTP cookies work.
- How to inspect cookies using Chrome Developer Tools.
- How client-side cookie values can be modified.
- Why applications should not trust client-controlled data.
- The importance of server-side validation.

## Conclusion

The challenge was solved by inspecting the application's cookies and modifying the `name` cookie until the correct value was found.

The key security lesson is:

> Never trust client-controlled values for security-sensitive decisions.
