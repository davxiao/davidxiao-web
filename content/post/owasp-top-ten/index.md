---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "OWASP Top Ten"
subtitle: "Open Web Application Security Project"
summary: "The OWASP Top 10 represents a broad consensus about the most common and critical security risks to web applications. It can be used as reference for web application security."
profile: false
authors:
  - david-xiao
categories:
  - CyberSecurity
tags:
  - owasp
  - applicationsecurity
date: 2020-10-13
lastmod: 2020-10-13
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

TL;DR

OWASP Top 10 Web Application Security Risks is a [security project](https://owasp.org/www-project-top-ten/) commonly referenced in web security and application security space.

The following table provides a quick summary of what are the Top 10 and how easy and effective to mitigate them using WAF (web application firewall).

😈 effective

🤔 possible with some considerations

😨 not considered broadly effective or as a primary mitigation

| ID |OWASP Item  |Mitigate with WAF  |
|:-:|:-:|:-:|
|1  |Injection  |😈  |
| 2 |  Broken Authentication| 😨 |
|3  |Sensitive Data Exposure  | 😨 |
|4  | XML External Entities XXE | 😈 |
|5  | Broken Access Control | 🤔 |
| 6 | Security Misconfiguration | 🤔 |
| 7 | Cross-Site Scripting XSS | 😈 |
| 8 | Insecure Deserialization | 🤔 |
| 9 | Using Components with Known Vulnerabilities |😨  |
| 10 | Insufficient Logging & Monitoring | 😨 |

## 1. Injection

Most common form is SQL injection. There are other forms of injection such as OS and LDAP injection. The attacker’s hostile data can trick the interpreter into executing unintended commands or accessing data without proper authorization.

WAF: It is usually effective in matching and mitigating such attacks.

## 2. Broken Authentication

To obtain these credentials, attackers either relying on vulnerabilities in the way client-server communication is implemented or targeting how tokens are generated, stored, transferred or invalidated by the application.

Attackers then use the credentials to impersonate legitimate users and make requests to your web applications using those tokens.

WAF is hard to mitigate this type of attacks in general. You might be able to add compromised or stolen tokens to a blacklist WAF rule.

## 3. Sensitive Data Exposure

Many web applications and APIs do not properly protect sensitive data, such as financial, healthcare, and PII. Attackers may steal or modify such weakly protected data to conduct credit card fraud, identity theft, or other crimes. Sensitive data may be compromised without extra protection, such as encryption at rest or in transit, and requires special precautions when exchanged with the browser.

WAF is typically hard to mitigate such kind of risks. For example, data has been decrypted from the connection level for WAF inspection so WAF has no impact on enforcing encryption hygiene.

## 4. XML External Entities (XXE)

Many older or poorly configured XML processors evaluate external entity references within XML documents. External entities can be used to disclose internal files using the file URI handler, internal file shares, internal port scanning, remote code execution, and denial of service attacks.

WAF can be helpful in mitigating this type of risk as long as the entity references can be matched as pattern.

## 5. Broken Access Control

It allows internal objects to be manipulated without the requestor’s access permissions being properly validated.

Depending on the specific workload, this can lead to exposure of unauthorized data, manipulation of internal web application state, path traversal, and local file inclusion.

WAF can be effective against certain types of such attack by matching dangerous HTTP request patterns that can indicate path traversal attempts, or remote and local file inclusion.

For example:

```text
https://example.com/download.php?file=..%2F..%2Fetc%2Fpasswd
```

## 6. Security Misconfiguration

This is commonly a result of insecure default configurations or enabling verbose error messages containing sensitive information.

To mitigate the risk, operating systems, frameworks, libraries and applications be securely configured and stay up-to-date in a timely fashion.

For example:

```text
Leaving default directory listings enabled on production web servers. This allows malicious users to browse for files that are hosted by the web server.
```

WAF can be leveraged to mitigate attempts as long as the HTTP request patterns that attempt to exploit them are recognizable.

These patterns, however, are also application-stack specific. They depend on the operating system, web server, frameworks, or the programming languages your code uses.

## 7. Cross-Site Scripting XSS

XSS flaws occur whenever an application includes untrusted data in a new web page without proper validation or escaping, or updates an existing web page with user-supplied data using a browser API that can create HTML or JavaScript.

Attackers leverage this flow to execute scripts in the victim’s browser which can retrieve user cookies or redirect the user to malicious sites.

**Stored XSS Attacks**

aka Persistent XSS. The injected script is permanently stored on the target server such as database or a comment field on a web page etc. The victim retrieves the malicious script when it visits the web page.

**Reflected XSS Attacks**

{{< youtube 9xyRKZbv5kQ >}}
<br>
aka Non-Persistent XSS. The injected script is reflected back to the same visitor via various forms such as in an error message, search result, or any other response when server response includes some or all of the input sent to the server as part of the request.

For example, when username `bob` log on failed, a vulnerable server produces error message that includes `bob` without proper escaping and safety checks.

An attacker can validate if a target website is vulnerable by constructing a deliberating input as username that includes executable JS code in `<script>...</script>` code block and attempting to submit it on the web site. If the JS code can be reflected back to the same visitor's browser and got executed, the validation is successful.

Combined with the delivery technique discussed in the next paragraphs, attacker would be able to exploit this vulnerability and pull off an attack.
  
Reflected attacks are delivered to victims via another means such as in a phishing e-mail. When a user is tricked into clicking on a malicious link from the email, it submits a specially crafted form with some JS code to obtain sensitive such as `document.cookie` to a website such as a online bank website that has XSS vulnerability.
  
The injected JS code travels to the vulnerable bank website, which in turn reflects the JS code back to the user’s browser as part of the HTML. The browser will execute the JS code and allow it to obtain the cookie because the JS appears to come from the same origin.

From OWASP:

> XSS flaws can be difficult to identify and remove from a web application. The best way to find flaws is to perform a security review of the code and search for all places where input from an HTTP request could possibly make its way into the HTML output.

WAF is relatively easy to mitigate this type of attack in common scenarios because they require specific key HTML tag names in the HTTP request.

## 8. Insecure Deserialization

Insecure deserialization often leads to remote code execution. Even if deserialization flaws do not result in remote code execution, they can be used to perform attacks, including replay attacks, injection attacks, and privilege escalation attacks.

WAF is relatively effective in mitigating this type of attacks but there are some considerations:

- It would require creating custom rules to match known patterns. These patterns are application specific and require more in-depth knowledge of those applications.

- Take into account the limits the service imposes on such rules.

## 9. Using Components with Known Vulnerabilities

Components, such as libraries, frameworks, and other software modules, run with the same privileges as the application. If a vulnerable component is exploited, such an attack can facilitate serious data loss or server takeover. Applications and APIs using components with known vulnerabilities may undermine application defenses and enable various attacks and impacts.

WAF is not considered the primary mitigating control for such risks. As secondary control, one can use WAF to filter and block HTTP requests to
functionality of such components that you aren’t using in your applications.
This helps reduce the attack surface of those components.

## 10. Insufficient Logging & Monitoring

Insufficient logging and monitoring, coupled with missing or ineffective integration with incident response, allows attackers to further attack systems, maintain persistence, pivot to more systems, and tamper, extract, or destroy data. Most breach studies show time to detect a breach is over 200 days, typically detected by external parties rather than internal processes or monitoring.

WAF is not considered the primary mitigating control for such risks. WAF can be produce its own logs for further consumption and analysis.
