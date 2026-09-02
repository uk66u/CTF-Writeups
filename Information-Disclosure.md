### Write-up: Solving a Web Information Disclosure Challenge

## Challenge Concept
* **Category:** Web Security / Enumeration
* **Objective:** Find the hidden flag by analyzing the source code and uncovering unlinked/hidden assets.

---

## Step-by-Step Solution

### Step 1: Inspecting the Page Source
Upon opening the main application link, a simple image gallery page was presented. To understand how the page was structured and check for any developer notes, I right-clicked and selected **View Page Source** (or pressed `Ctrl + U`).

### Step 2: Uncovering Hidden Comments
Inside the HTML code, a series of nested HTML comments contained hidden image paths that were not displayed on the front end:
&lt;!-- img src="imgs/cyber.jpg" 
    &lt;!-- img src="imgs/cyber1.jpg" 
        &lt;!-- img src="img/cyber2.jpg" 
            &lt;!-- img src="imgs/cyber3.jpg" --&gt;
        --&gt;
    --&gt;
--&gt;

### Step 3: Enumerating Hidden Assets
These hidden paths were systematically tested by appending them directly to the base URL of the web application (e.g., checking `.../img/cyber2.jpg`).

### Step 4: Extracting the Flag
Navigating to the correct hidden image path successfully loaded the asset, which revealed the flag directly on the image.

---

## Key Takeaway
This challenge serves as a reminder of the risks associated with **Information Disclosure**. Developers should always clean up unused assets, debug notes, and hidden source comments before deploying applications to production, as attackers can easily leverage them to find sensitive information.

#CyberSecurity #CTF #WebSecurity #CaptureTheFlag #InfoSec #AppSec
