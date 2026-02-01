# OWASP Top 10 Mock Report
A mock report testing the 2025 OWASP Top 10 application risks, featuring exploitation walkthroughs, impact, and remediation sections.

## Notes

### Get Setup

#### Command Line
1. Launch Kali machine in VMware
2. `sudo apt update`
3. `sudo apt install docker.io -y`
4. `sudo systemctl start docker`
5. `sudo systemctl enable docker`
6. `sudo docker pull bkimminich/juice-shop`
7. `sudo docker run -d -p 3000:3000 bkimminich/juice-shop`

#### Open Juice Shop in Browser
8. Burp Suite > Proxy > Intercept > Open browser 
9. `http://localhost:3000`

### Testing

#### A01 - Broken Access Control
- Exploit an IDOR (Insecure Direct Object Reference) vulnerability

Steps:
On website
1. Made a user account on the website
2. Added 1 item to my basket
3. Viewed basket

On Burp Suite
4. Intercepted `/rest/basket/7` which has the `id` of 7 within the packet
5. Send the packet to repeater
6. Changed `/rest/basket/7` to `/rest/basket/1` and sent
7. Response shows a status code 200 (OK) meaning it was successful. Viewing the packet shows a basket with 3 items (someone elses basket)
8. Repeating the website steps with interceptor on allowed me to catch the `/rest/basket/7` packet, edit it to `/rest/basket/1` and send it to the website, which displays the 3 items in the browser.

- [ ] Step 6. & 7: 2 screenshots, `/rest/basket/7` and `/rest/basket/1` requests and responses side-by-side
- [ ] Loom video demonstrating step 8.



#### A02 - Security Misconfiguration
- A05 included in this too?
- Before A05, the `/#/administration` gave a 403 error, meaning it is discoverable (confirming its existence) but not accessible. Still a misconfiguration
- Dirbuster to expose directories visible and accessible on client-side?
- After A05, I can access the `/#/administration` panel from the url bar

- /robots.txt reveal /ftp, which allows for file navigation, veiwing and downloading of files
- Sensitive file types include .kdbx, .bak, .pyc
- No authenitcation required
- Can download .kdbx file for offline decryption (Most likely contains all passwords)
 

- [ ] Screenshot of admin panel


#### A03 - Software Supply Chain Failures
- Outdated software / os / dependencies? -> vulnerability -> exploit
- Banner grabbing?


#### A04 - Cryptographic Failures
- Once A05 is done, can look at burp suite request when logging in.
- Reveals a JWT token
- Use jwt.io to decode the token
- Take the password hash from the token, decrypt offline with crackstation or Hashcat
- Uses weak md5 hashing algorithm 

#### A05 - Injection
On Login page:
1. Use `admin@juice-sh.op'--` as the username and the password as anything
2. Logs in. Why? '-- comments the rest of the SQL query to bypass authentication

- [ ] Loom of SQL injection

#### A06 - Insecure Design



#### A07 - Authentication Failures



#### A08 - Software or Data Integrity Failures



#### A09 - Security Logging and Alerting Failures



#### A10 - Mishandling of Exceptional Conditions



