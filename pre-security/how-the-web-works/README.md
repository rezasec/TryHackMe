# How the Web Works

In this section I learned what happens behind the scenes when you visit a website. I went through four modules: **DNS in Detail**, **HTTP in Detail**, **How Websites Work**, and **Putting It All Together**. 

---

## 1. DNS in Detail

### What is DNS?
DNS (Domain Name System) is like the internet's phonebook. It translates domain names like `google.com` into IP addresses that computers use to talk to each other.

### Key DNS Concepts:
- **Domain**: A human readable address like `tryhackme.com`
- **Nameserver**: A server that stores DNS records
- **DNS Records**:
  - **A record**: Maps a domain to an IPv4 address
  - **AAAA record**: Maps to an IPv6 address
  - **CNAME**: Alias for another domain name
  - **MX**: Mail exchange info
  - **NS**: Lists the authoritative name servers
  - **TXT**: Usually holds SPF/DKIM info or human readable text

### DNS Lookup Flow:
1. Your computer checks its cache
2. If not found, it asks a **recursive DNS server** which is usually your ISP
3. If still not found, it queries:
   - **Root DNS server** → tells it which TLD server to ask
   - **TLD server** → tells it which **authoritative server** has the answer
   - **Authoritative server** → gives the final IP

The response is cached based on the TTL (time to live).

Knowing how DNS works helps in analyzing suspicious domain lookups. DNS tunneling and exfiltration rely on this system.

---

## 2. HTTP in Detail

### HTTP vs HTTPS
- **HTTP** is the protocol used for fetching web pages
- **HTTPS** is the secure version of HTTP 

### HTTP Methods:
- **GET**: Request data
- **POST**: Submit data
- **PUT**: Update data
- **DELETE**: Delete data

You don’t usually type these into the terminal. But tools like Burp Suite or cURL simulate them for testing.

### Status Codes:
- **200**: OK
- **201**: Created
- **301/302**: Redirects
- **400**: Bad request
- **401**: Unauthorized
- **403**: Forbidden
- **404**: Not found
- **500/503**: Server errors

### Headers:
**Request Headers:**
- `Host`: Domain you're trying to reach
- `User Agent`: Browser info
- `Content Length`: Size of data sent
- `Accept Encoding`: Compression types supported
- `Cookie`: Session or user info

**Response Headers:**
- `Set Cookie`: Saves data on your machine
- `Cache Control`: Tells how long data is valid
- `Content type`: Type of file being returned
- `Content Encoding`: How it's compressed

### Cookies:
Cookies are small pieces of data saved on your computer. The server uses them to remember who you are, especially for things like logins. Stored in headers like `Set Cookie` and sent back in `Cookie` headers.

Understanding how headers, cookies, and status codes work is key to spotting session hijacking, CSRF, and header based attacks.

---

## 3. How Websites Work

Websites have two major components:
- **Frontend**: What you see (HTML, CSS, JavaScript)
- **Backend**: What happens behind the scenes (logic, database calls)

### HTML Basics:
HTML structures the content of a page using tags:
- `<h1>`: Header
- `<p>`: Paragraph
- `<img>`: Image
- `<a>`: Link
- Attributes like `id`, `class`, and `src` define how content is styled or loaded

### JavaScript:
Used for interactivity. It can change content dynamically based on user actions.

Example:
```html
<button onclick='document.getElementById("demo").innerHTML = "Button Clicked";'>Click Me</button>
```

### Sensitive Data Exposure:
Sometimes, developers accidentally leave comments or variables in the HTML or JS that reveal passwords, API keys, or hidden paths. Always check page source code when testing.

### HTML Injection:
If user input is not sanitized, you can inject HTML code into the page. For example:
```html
<a href="http://malicious.com">Click here</a>
```
Can be used to trick users or redirect them. Developers should always sanitize user input.

---

## 4. Putting It All Together

When you type in a website URL:
1. DNS resolves it to an IP
2. Your browser sends an HTTPs request
3. The web server responds with content (HTML, CSS, JS, images)

### Extra Components:
- **Load Balancer**: Spreads traffic across multiple servers
- **CDN (Content Delivery Network)**: Delivers static content from locations close to the user
- **Database**: Stores dynamic content like user profiles, blog posts, etc.
- **WAF (Web Application Firewall)**: Filters out malicious traffic before it hits the server

### Web Server Software:
- **Apache / Nginx / IIS / Node.js**
- Serves files from a root directory like `/var/www/html`

### Virtual Hosts:
- Let one server host multiple websites
- Uses the `Host:` header to decide which config to use

### Static vs Dynamic:
- **Static**: Fixed content like images, CSS
- **Dynamic**: Built on the fly by backend code (PHP, Python, etc.)

---
