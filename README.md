# Reflected-XSS-testasp.vulnweb.com
Reflected Cross-Site Scripting (XSS) is a common web application vulnerability that occurs when user input is taken from an HTTP request and immediately reflected in the server’s response without proper validation or encoding. Attackers exploit this flaw by crafting a malicious URL that includes a JavaScript payload in a parameter.





1.Vulnerability summary:

Type: Reflected Cross-Site Scripting (XSS)

Location: Search.asp (user-supplied input reflected in the response without proper encoding/validation)

Attack vector: Crafted URL / POST data causes attacker-supplied script to execute in victim's browser when they open the URL.

Impact: Theft of cookies/session tokens (if HttpOnly not set), UI redressing, phishing, and arbitrary JavaScript execution in victim context.

Severity: Medium to High (depends on surrounding controls: HttpOnly, SameSite, CSP).






2.Responsible disclosure note:

This PoC is provided for educational / authorized security testing only. vulnweb.com is a deliberately vulnerable testing site operated by Acunetix — use responsibly. Do not test sites you do not have explicit permission to test.





3.Affected resource:

URL: http://testasp.vulnweb.com/Search.asp

Location: Search input field (query parameter Search reflected into response)





4.Proof of concept (PoC):

Browser (address bar)
http://testasp.vulnweb.com/Search.asp?Search=abc%3Cscript%3Ealert(1)%3C%2Fscript%3E

Open the URL. If reflected unencoded, an alert(1) will appear confirming reflected XSS.





![Image](https://github.com/user-attachments/assets/7f0397ab-ea7d-4b92-82c3-806bdad9e7c1)




5.Browser (form input):

1.Go to http://testasp.vulnweb.com/Search.asp.

2.Paste <script>alert(1)</script> into the site’s search bar and submit.

3.If echoed unencoded in the response, the alert triggers.

Command line (curl):
curl -v "http://testasp.vulnweb.com/Search.asp?Search=%3Cscript%3Ealert(1)%3C%2Fscript%3E"


![Image](https://github.com/user-attachments/assets/f2ebbfa1-fcc6-41a3-846a-336c52bd2330)



6.Technical detail / root cause

User-supplied input from the search parameter is rendered into the HTML response without proper context-sensitive encoding. HTML special characters (<, >, &, ", ') are not escaped, so injected markup is executed by the browser.











