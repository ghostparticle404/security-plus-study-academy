# Day 32 — Web Application Vulnerabilities

**Week 07 | Vulnerability Management**

A website can use HTTPS, run on a patched server, and still expose confidential information. Encryption protects information traveling between a browser and a server, but it does not guarantee that the application makes safe decisions about that information. Today, we examine weaknesses in the way web applications process input, establish identity, enforce permissions, and maintain sessions.

> **CompTIA Security+ SY0-701** · **Week 07, Lesson 2 of 5**  
> **Level:** Beginner to intermediate · **Study time:** 80–105 minutes  
> **Previous:** [Day 31 — Vulnerability Sources and Misconfiguration](day-31-vulnerability-sources-and-misconfiguration.md)  
> **Next:** Day 33

## Security+ exam alignment

**SY0-701 — Domain 2: Threats, Vulnerabilities, and Mitigations**

- **2.3:** Explain various types of vulnerabilities.
- **2.4:** Given a scenario, analyze indicators of malicious activity.
- **2.5:** Explain the purpose of mitigation techniques used to secure the enterprise.
- **4.6 (supporting):** Given a scenario, implement and maintain identity and access management.

The primary exam focus is recognizing SQL injection, cross-site scripting (XSS), application attacks, session replay, and appropriate mitigations. The HTTP examples and additional web security categories provide context for understanding these concepts; they are not a requirement to memorize programming syntax.

<a id="learning-objectives"></a>

## 1. Learning objectives

By the end of this lesson, you should be able to:

- Explain how untrusted input crosses web application trust boundaries.
- Distinguish SQL injection, command injection, and cross-site scripting.
- Compare reflected, stored, and DOM-based XSS.
- Distinguish authentication failures from authorization failures.
- Explain session hijacking, session fixation, replay, and cross-site request forgery.
- Match common web application weaknesses to appropriate preventive controls.
- Analyze fictional HTTP requests and identify potential weaknesses without assuming exploitation has occurred.

<a id="why-this-topic-matters"></a>

## 2. The big idea: Trust boundaries matter

Web applications handle banking, healthcare, email, education, and business records. Their interfaces are designed to accept information from users, making them particularly important trust boundaries.

Imagine a receptionist who receives written requests. A legitimate request might ask for a copy of your own records. An unsafe process might allow a visitor to write instructions that the receptionist treats as management orders, or request another person's records without proving authorization. The problem is not that requests exist; it is that the organization fails to distinguish data, instructions, identity, and permission.

As we learned in [Day 31](day-31-vulnerability-sources-and-misconfiguration.md), a weakness is not proof that an attack has occurred. Web security analysts must distinguish vulnerable behavior, suspicious requests, and confirmed impact.

<a id="1-how-web-applications-process-requests"></a>

## 3. How web applications process requests

A web application typically involves a browser, a web server or reverse proxy, application logic, and one or more data stores. These components may run on separate systems or within the same service.

```text
Browser
   | HTTP request (usually protected by TLS)
   v
Web server / reverse proxy
   |
   v
Application logic ----> Database or other service
   |
   v
HTTP response ----> Browser
```

Consider this fictional request:

```http
GET /catalog?category=books HTTP/1.1
Host: shop.example.test
Accept: application/json
```

The application reads the category, retrieves appropriate records, and returns a response. The browser controls the value of `category`; therefore, the application must treat it as untrusted data.

Untrusted input can arrive through URL parameters, request bodies, cookies, headers, uploaded files, and data retrieved from external systems. Even information stored in an internal database may have originated from an untrusted user.

A **trust boundary** is a point where data or requests cross between environments with different security assumptions. Crossing a boundary requires appropriate checks. HTTPS authenticates the server when certificates are correctly validated and protects transport confidentiality and integrity, but does not make user-supplied input trustworthy.

### Client-side and server-side checks

A browser may reject an invalid email address or prevent a user from selecting an unavailable option. These checks improve usability, but a client can send HTTP requests independently of the displayed interface.

Security decisions must be enforced by the server or another trusted enforcement component. Hiding an administrative button is not equivalent to preventing unauthorized administrative requests.

<a id="2-injection-vulnerabilities"></a>

## 4. Injection vulnerabilities

**Injection** occurs when untrusted data is interpreted as instructions by a component that processes a command, query, or other structured language. The underlying problem is a failure to maintain the boundary between data and executable instructions.

### SQL injection

Structured Query Language (SQL) is used to interact with relational databases. A web application may query a database to retrieve a product or locate a user record.

The following simplified Python-like pseudocode illustrates an unsafe design. It is not intended to be executed.

```python
query = "SELECT title FROM books WHERE category = '" + category + "'"
database.execute(query)
```

The application constructs a query by concatenating a user-controlled value into SQL text. If that value changes the structure of the resulting query, the database may interpret part of the input as SQL instructions rather than as a category name.

Possible consequences include unauthorized reading or modification of records, deletion of data, and authentication bypass where vulnerable queries participate in login decisions. Actual impact depends on database permissions, query behavior, and other controls.

A safer design uses a **parameterized query** (also called a prepared statement with bound parameters):

```python
# Illustrative SQLite-style parameter binding
cursor.execute(
    "SELECT title FROM books WHERE category = ?",
    (category,)
)
```

Here, the query structure and parameter value are supplied separately. The database interface treats the category as a value rather than as SQL syntax. Placeholder syntax varies between database libraries.

Parameter binding is intended for values; dynamic identifiers such as table names generally require a separate safe design, such as mapping a fixed allowlist to known identifiers.

Input validation, least-privilege database accounts, and generic error messages provide additional protection. Input validation alone is not a reliable substitute for parameterization.

### Command injection

Command injection occurs when untrusted input alters an operating system command executed by an application. For example, a reporting service might pass a user-provided filename to a shell command that generates a report.

If the application combines the filename with shell instructions unsafely, the operating system may interpret part of the filename as a command. The consequences depend on the privileges of the application process.

Defenders should avoid invoking a shell when a direct library or API can perform the operation. Where subprocesses are necessary, use fixed executables, structured arguments, strict validation, and least privilege. Avoiding shell interpretation does not automatically eliminate all risks associated with unsafe arguments.

| Feature | SQL injection | Command injection |
| --- | --- | --- |
| Target interpreter | Database query engine | Operating system command processor |
| Typical weakness | Unsafe query construction | Unsafe command construction |
| Potential impact | Unauthorized database operations | Unauthorized system operations |
| Primary prevention | Parameterized queries | Avoid shell interpretation and unsafe command construction |

<a id="3-cross-site-scripting-xss"></a>

## 5. Cross-site scripting (XSS)

Cross-site scripting occurs when an application causes untrusted content to be interpreted as executable script in a user's browser. Unlike SQL injection, the principal execution environment is the browser, not the database.

A browser normally allows scripts associated with a website to interact with resources available to that website's origin. An **origin** is defined by the scheme, host, and port. If untrusted script executes within a trusted origin, it may inherit access available to scripts in that origin.

Consider a fictional search page that displays a search term in an HTML response. If the application inserts the term directly into HTML without appropriate encoding, the browser may interpret markup supplied by a user rather than displaying it as text.

A harmless illustration uses the text `<b>Training</b>`. In a plain-text display, the user should see the angle brackets and letters. If interpreted as HTML, the browser instead displays a bold word. This demonstrates the distinction between data and markup; rendering harmless markup alone does not establish that executable script is possible.

### Reflected XSS

Reflected XSS occurs when untrusted request data is included in an immediate response and interpreted as script by the browser. A vulnerable search or error page is a common example. The malicious content need not be permanently stored by the application.

### Stored XSS

Stored XSS occurs when untrusted content is saved by the application and later delivered to users in a way that allows script execution. A vulnerable comment system, support ticket, or profile field may expose multiple users when they view stored content.

### DOM-based XSS

The Document Object Model (DOM) is the browser's representation of a document. Client-side JavaScript can read and modify it. DOM-based XSS occurs when unsafe client-side processing moves untrusted data into a browser operation that interprets it as executable content.

For example, compare these illustrative JavaScript operations:

```javascript
// Unsafe for displaying untrusted plain text
result.innerHTML = userText;

// Appropriate for displaying plain text
result.textContent = userText;
```

`innerHTML` parses its input as markup. `textContent` inserts text without parsing it as HTML. The appropriate safe operation depends on the intended context.

DOM-based XSS describes where the unsafe processing occurs. It can overlap with stored or reflected delivery mechanisms; these categories are not always mutually exclusive.

| XSS category | Defining characteristic |
| --- | --- |
| Reflected | Untrusted request content is returned in a response. |
| Stored | Untrusted content persists and is later delivered to users. |
| DOM-based | Unsafe client-side processing creates the executable context. |

### Impact and prevention

XSS may allow unauthorized actions using a victim's authenticated session, manipulation of displayed content, or access to data available to scripts in the affected origin. An HttpOnly cookie cannot normally be read directly by JavaScript, but XSS can still enable authenticated requests and other harmful actions.

Primary defenses include context-appropriate output encoding, safe templating, and safe DOM APIs. When an application intentionally accepts rich HTML, a maintained HTML sanitizer is needed. Encoding for HTML text is not necessarily appropriate for JavaScript, URLs, or HTML attributes.

A **Content Security Policy (CSP)** can restrict permitted script execution and provide defense in depth. It does not replace correcting unsafe rendering behavior.

<a id="4-authentication-and-authorization-weaknesses"></a>

## 6. Authentication and authorization weaknesses

**Authentication** establishes who a user is. **Authorization** determines what that identity is permitted to do. A user may be correctly authenticated and still be unauthorized to access a particular resource.

### Authentication weaknesses

Common authentication weaknesses include weak password policies, missing multifactor authentication (MFA), inadequate rate limiting, insecure account recovery, and unsafe storage of passwords.

Credential stuffing uses previously obtained username and password combinations against other services. Password spraying attempts a small number of commonly used passwords across multiple accounts. Both exploit weaknesses in authentication defenses, but they differ in their patterns.

Controls include MFA, rate limiting, monitoring, secure recovery procedures, and salted password hashing with an appropriate password-hashing algorithm. Phishing-resistant MFA is preferable where practical. Account lockout policies should also consider the possibility of denial of service against legitimate users.

### Broken access control

Consider a fictional document portal:

```http
GET /api/documents/104 HTTP/1.1
Host: portal.example.test
Cookie: session=TRAINING_SESSION
```

The session identifies an authenticated user. The number identifies a requested document. The application must verify that the authenticated user is permitted to access that specific document before returning it.

An **insecure direct object reference (IDOR)** is a form of broken access control in which an application exposes a reference to an object without adequately checking whether the requester may access it. In API contexts, the broader category is often called broken object level authorization (BOLA).

Sequential identifiers are not inherently vulnerabilities. Replacing them with random identifiers may make guessing harder, but does not replace authorization checks.

**Horizontal privilege escalation** involves gaining access to resources or capabilities belonging to another user at a similar privilege level. **Vertical privilege escalation** involves gaining capabilities associated with a higher privilege level, such as administrative functions.

Defenders should enforce authorization on every relevant request, deny access by default, apply least privilege, and test both object-level and function-level permissions.

<a id="5-sessions-cookies-and-tokens"></a>

## 7. Sessions, cookies, and tokens

HTTP is fundamentally stateless: individual requests do not inherently remember a user's previous login. Applications maintain continuity using session identifiers or tokens.

A typical server-side session works as follows:

1. A user successfully authenticates.
2. The server creates a session and issues an unpredictable identifier.
3. The browser returns that identifier with subsequent requests.
4. The server checks the session and applies authorization rules.
5. The session expires or is invalidated when appropriate.

A cookie is a browser mechanism for storing and sending data according to cookie rules. A session identifier is a value used to associate requests with a session. A token may carry claims or reference server-side state. These concepts are related but not interchangeable.

An illustrative response header is:

```http
Set-Cookie: session=TRAINING_ONLY; Secure; HttpOnly; SameSite=Lax; Path=/
```

The value is a fictional placeholder, not a suitable production session identifier.

| Attribute | Purpose | Limitation |
| --- | --- | --- |
| Secure | Restricts cookie transmission to secure connections under normal browser rules. | Does not correct application vulnerabilities. |
| HttpOnly | Prevents ordinary JavaScript access to the cookie. | Does not prevent all XSS impact or automatic cookie transmission. |
| SameSite | Restricts cookie sending in certain cross-site contexts. | Does not replace all CSRF defenses. |
| Path | Controls the request paths for which the browser sends the cookie. | Is not a reliable security isolation boundary. |

Cookies should also have appropriate domain scope and expiration. Session expiration and revocation must be enforced by the server, not merely by removing a cookie from the browser.

### Session hijacking, fixation, and replay

**Session hijacking** occurs when an unauthorized party takes over an existing authenticated session, often through possession of a valid session credential.

**Session fixation** occurs when an application allows a session identifier known before authentication to remain valid after authentication, enabling an unauthorized party who knows that identifier to use the authenticated session. Regenerating the session identifier at login and privilege changes helps prevent fixation.

**Replay** involves reusing previously captured authentication material or messages to obtain an unauthorized result. Reusing a stolen bearer session token can constitute both session hijacking and replay. A bearer credential generally grants access to whoever presents it, subject to the application's validation rules.

Controls include HTTPS, unpredictable session identifiers, short and appropriate lifetimes, session rotation, revocation, and protection against credential leakage. Protocols that require freshness may also use nonces, timestamps, or challenge-response mechanisms.

Signed tokens protect integrity when correctly validated; signatures do not inherently encrypt token contents or prevent replay. Token-based systems must validate signatures, expiration, issuer, audience, and other applicable claims.

<a id="6-cross-site-request-forgery-csrf"></a>

## 8. Cross-site request forgery (CSRF)

CSRF occurs when a user's browser is induced to send an unwanted request to a site where the user is already authenticated, and that site accepts the request without adequate verification of its legitimacy.

Imagine a user logged into a fictional account portal. A separate website causes the browser to submit an account-setting change. If the browser automatically includes the portal's session cookie and the portal accepts the request without appropriate CSRF defenses, the unwanted change may succeed.

CSRF does not necessarily require the attacker to read the victim's session cookie or the response. The weakness concerns the application's acceptance of a request carrying ambient authentication credentials.

Defenses include server-validated anti-CSRF tokens, appropriate SameSite cookie settings, and origin verification where applicable. Sensitive state-changing operations should not use GET requests. Using POST alone is not sufficient to prevent CSRF.

| Concept | Core distinction |
| --- | --- |
| XSS | Untrusted script executes in a trusted browser origin. |
| CSRF | A browser sends an unwanted authenticated request. |
| Session hijacking | An unauthorized party takes over a valid session. |
| Session fixation | A known pre-login identifier remains valid after login. |
| Replay | Previously captured material is reused. |

XSS may defeat many CSRF defenses because script executing within the trusted origin may be able to access tokens or submit same-origin requests. Preventing both weaknesses remains important.

<a id="7-additional-web-weaknesses-and-defense-in-depth"></a>

## 9. Additional web weaknesses and defense in depth

The following concepts broaden your ability to recognize application exposure. Focus first on the core exam concepts above.

| Weakness | Description | Appropriate control |
| --- | --- | --- |
| Directory traversal | Untrusted path input permits access outside an intended directory. | Restrict file selection to approved resources and enforce resolved-path boundaries. |
| Insecure file upload | An application accepts files that may be processed or served unsafely. | Validate allowed types and sizes, isolate storage, and prevent execution. |
| Security misconfiguration | Debugging interfaces, default credentials, or excessive permissions remain enabled. | Secure baselines, configuration review, and least privilege. |
| Vulnerable dependencies | A third-party library contains a security flaw. | Inventory dependencies and apply tested updates. |
| Sensitive information exposure | Secrets or confidential data appear in responses, logs, or inappropriate storage. | Minimize data, restrict access, and protect secrets. |

A **web application firewall (WAF)** inspects HTTP traffic and may block requests matching suspicious patterns or policies. A WAF can reduce exposure while developers correct a weakness, but cannot reliably compensate for every insecure business-logic or authorization decision.

Secure development combines input validation, safe interpreter interfaces, output encoding, access control, secure session handling, dependency management, and testing. These controls address different failure modes and are not interchangeable.

<a id="8-recognizing-indicators-in-security-operations"></a>

## 10. Recognizing indicators in security operations

Security teams may examine reverse proxy logs, application logs, authentication events, WAF alerts, and database audit records. Correlation helps determine whether suspicious activity reached vulnerable functionality and produced an unauthorized result.

| Observation | Possible interpretation | Useful follow-up |
| --- | --- | --- |
| Repeated database syntax errors associated with request parameters | Possible injection probing or application defects | Correlate requests with application errors and database audit events. |
| Unexpected markup in stored comments | Possible unsafe rendering or legitimate formatting | Review rendering context and sanitization controls. |
| Many failed logins across different accounts | Possible password spraying or another authentication issue | Examine timing, source patterns, and successful logins. |
| Successful requests for resources outside a user's expected permissions | Possible broken access control | Compare object ownership, permissions, and response records. |
| Session activity from inconsistent environments | Possible session misuse or legitimate mobility | Review session lifecycle, device context, and user activity. |

A status code alone does not prove an attack succeeded. A `200 OK` response might contain a generic error page; a `500 Internal Server Error` might result from a programming defect. Logs should avoid recording passwords, complete session tokens, and unnecessary sensitive data.

<a id="10-common-misconceptions-and-exam-traps"></a>

## 11. Common misconceptions and exam traps

| Misconception | Correct understanding |
| --- | --- |
| HTTPS prevents SQL injection. | TLS protects transport, not query construction. |
| Input validation alone prevents XSS. | Safe rendering and context-appropriate output encoding are essential. |
| SQL injection and XSS execute in the same place. | SQL injection targets the database interpreter; XSS targets the browser. |
| A logged-in user can access any application resource. | Authorization must be checked independently. |
| HttpOnly prevents all session abuse. | It restricts script access to cookies, not all authenticated actions. |
| POST requests cannot be forged. | POST alone does not prevent CSRF. |
| Random object identifiers replace authorization. | Unpredictable identifiers are not permission checks. |
| A WAF fixes vulnerable source code. | It may mitigate exposure but does not remove the underlying defect. |

<a id="11-key-terminology-and-takeaways"></a>

## 12. Key terminology and takeaways

| Term | Meaning |
| --- | --- |
| Trust boundary | A transition between different security assumptions. |
| Injection | Untrusted data interpreted as instructions. |
| Parameterized query | A query that binds values separately from SQL structure. |
| XSS | Untrusted script execution in a browser's trusted web context. |
| Output encoding | Representing data safely for a particular output context. |
| Authentication | Establishing identity. |
| Authorization | Determining permitted actions and resources. |
| IDOR | Object access without adequate authorization checks. |
| Session fixation | Retention of a known pre-authentication session identifier. |
| CSRF | Acceptance of an unwanted authenticated browser request. |
| WAF | A control that inspects and filters web application traffic. |

Remember the interpreter, the trust boundary, and the missing control. Identifying those three elements is more useful than memorizing a collection of attack names.

## 13. Worked case study — A secure connection, an insecure portal

A fictional law firm uses HTTPS and MFA for its client document portal. During an authorized review with synthetic records, a tester discovers that the server checks login status but does not check whether the requested document belongs to the authenticated client.

| Question | Analyst reasoning |
| --- | --- |
| Which control worked? | TLS protected transport; MFA helped establish identity. |
| Which boundary failed? | The application failed to enforce object-level authorization. |
| What is the weakness? | Broken access control, potentially IDOR/BOLA. |
| What is the potential impact? | Horizontal privilege escalation and disclosure of another client's records. |
| What is established? | The controlled test demonstrates missing authorization; historical disclosure is not established. |
| What should change? | Enforce object permissions on every request, test with separate fictional accounts, and review access logs. |

**Memory hook:** Encryption protects the journey; authorization controls the destination.

## 14. Guided notes — Write first, reveal later

Complete the table from memory before revealing the model answers.

| Prompt | Your notes |
| --- | --- |
| Why is HTTPS insufficient to prevent injection? | |
| What distinguishes SQL injection from command injection? | |
| What distinguishes reflected, stored, and DOM-based XSS? | |
| Why is authentication insufficient for object access? | |
| What prevents session fixation? | |
| What do Secure, HttpOnly, and SameSite do? | |
| How does CSRF differ from XSS? | |
| Why is a WAF alert not proof of compromise? | |

<details markdown="1">
<summary>Reveal model guided notes</summary>

- **Why is HTTPS insufficient to prevent injection?** TLS protects transport, not how an interpreter processes input.
- **What distinguishes SQL injection from command injection?** The former targets a database interpreter; the latter targets an operating system command processor.
- **What distinguishes reflected, stored, and DOM-based XSS?** Immediate response, persistent content, and unsafe client-side processing, respectively; categories can overlap.
- **Why is authentication insufficient for object access?** The server must independently verify permission for the requested object.
- **What prevents session fixation?** Rotate the session identifier at login and invalidate the old identifier.
- **What do Secure, HttpOnly, and SameSite do?** Restrict cookie transport, ordinary script access, and certain cross-site sending, respectively.
- **How does CSRF differ from XSS?** CSRF induces an unwanted authenticated request; XSS executes untrusted script in a trusted origin.
- **Why is a WAF alert not proof of compromise?** It reports a detection; application and database evidence are needed to establish impact.

</details>

<a id="9-practical-activity-analyze-safe-example-requests"></a>

## 15. Hands-on lab — Analyze safe example requests

**Estimated time:** 20 minutes.

**Materials:** The fictional requests and observations below. No live website, network connection, or attack tools are required. The `.test` domain is reserved for testing. Do not send these examples to external systems.

For each case, identify the likely weakness, explain the trust boundary involved, recommend a primary mitigation, and state what additional evidence would be needed to confirm impact.

### Part A — Input and authorization boundaries

#### Case A — Catalog search

```http
GET /catalog?category=children%27s-books HTTP/1.1
Host: academy.example.test
```

A developer reports that this ordinary category value produces a database syntax error. Code review reveals that category values are concatenated into SQL strings.

<details markdown="1">
<summary>Reveal analysis</summary>

The encoded apostrophe (`%27`) is ordinary user data. Unsafe query concatenation suggests SQL injection exposure. Use parameterized queries and least-privilege database access. The error does not establish unauthorized database access; review code and database audit evidence.

</details>

#### Case B — Discussion board

```http
POST /comments HTTP/1.1
Host: academy.example.test
Content-Type: application/x-www-form-urlencoded

comment=%3Cb%3ETraining%3C%2Fb%3E
```

The application saves the comment and later renders the word Training in bold, although comments are intended to be plain text.

<details markdown="1">
<summary>Reveal analysis</summary>

The application appears to interpret stored user data as HTML. This indicates an unsafe rendering boundary and possible stored XSS exposure, but harmless formatting does not prove script execution. Use context-appropriate output encoding or safe text rendering. Review the rendering implementation and authorized test results.

</details>

#### Case C — Document portal

```http
GET /api/documents/104 HTTP/1.1
Host: academy.example.test
Cookie: session=TRAINING_SESSION
```

The application verifies that the user is logged in but does not check document ownership or sharing permissions.

<details markdown="1">
<summary>Reveal analysis</summary>

This is broken object-level authorization and potentially IDOR. Authentication is not enough. Enforce resource-level permissions on the server for every request. Confirm actual disclosure through authorized tests using fictional accounts and records.

</details>

### Part B — Session and request integrity

#### Case D — Login lifecycle

A fictional application's session identifier remains unchanged when an anonymous visitor logs in. The identifier is known to another party before authentication.

<details markdown="1">
<summary>Reveal analysis</summary>

This suggests session fixation. Rotate the identifier on successful authentication and invalidate the previous identifier. Confirm whether the old identifier actually grants authenticated access using an authorized test environment.

</details>

#### Case E — Account settings

```http
POST /account/preferences HTTP/1.1
Host: academy.example.test
Cookie: session=TRAINING_SESSION
Content-Type: application/x-www-form-urlencoded

newsletter=off
```

The fictional server accepts state-changing requests authenticated by cookies without checking an anti-CSRF token or request origin. The cookie is configured to permit cross-site sending.

<details markdown="1">
<summary>Reveal analysis</summary>

The application may be vulnerable to CSRF. A browser could submit an unwanted request with the user's existing cookie. Implement server-validated anti-CSRF tokens and appropriate cookie and origin controls. Confirm whether browser behavior and server validation actually permit the unwanted change.

</details>

<a id="12-practice-questions"></a>

## 16. Eight original Security+-style questions

**1.** A developer constructs database queries by appending values received from a web form. Which change most directly addresses the injection weakness?

A. Enable HTTPS on the server.  
B. Use parameterized database queries.  
C. Add a SameSite attribute to cookies.  
D. Increase the session timeout.  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: B.** Parameterized queries separate data values from SQL structure. HTTPS protects transport; cookie attributes and timeouts address other security concerns.

</details>

**2.** A support portal saves customer messages and later displays them to employees. Unsafe rendering allows script execution when an employee opens a message. Which weakness is most directly described?

A. Reflected XSS  
B. Command injection  
C. CSRF  
D. Stored XSS  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: D.** The content persists in the application and executes when another user views it. Reflected XSS typically involves content returned from a request without requiring persistent storage.

</details>

**3.** An authenticated employee can retrieve another employee's private report because the server checks login status but not report permissions. Which control is missing?

A. Object-level authorization  
B. Transport encryption  
C. Password hashing  
D. Output encoding  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: A.** The server establishes identity but fails to verify permission to access the specific object.

</details>

**4.** A session identifier known before login continues to grant access after the user authenticates. Which action most directly mitigates this weakness?

A. Increase cookie expiration time.  
B. Encode all HTML output.  
C. Regenerate the session identifier at login and invalidate the old one.  
D. Parameterize database queries.  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: C.** Session rotation prevents a known pre-authentication identifier from remaining usable as an authenticated credential. This addresses session fixation.

</details>

**5.** An authenticated user's browser submits an unwanted account change initiated by another website. Which control most directly verifies that the state-changing request includes the application's expected anti-forgery value?

A. Database encryption  
B. Server-validated anti-CSRF token  
C. HttpOnly cookie attribute  
D. Password complexity policy  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: B.** A correctly implemented anti-CSRF token helps distinguish legitimate application requests from forged requests. HttpOnly does not prevent automatic cookie transmission.

</details>

**6.** A WAF reports a suspicious request followed by a database error. What should an analyst conclude first?

A. Confidential records were definitely stolen.  
B. The WAF permanently corrected the application defect.  
C. The request proves a zero-day exploit was used.  
D. The events warrant investigation but do not independently prove successful exploitation.  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: D.** Correlate the request with application behavior, database events, and evidence of unauthorized access or modification before determining impact.

</details>

**7.** A client-side script inserts untrusted text using `innerHTML`. Which change most directly addresses unsafe rendering when only plain text is needed?

A. Increase the cookie lifetime  
B. Disable database logging  
C. Use `textContent` instead  
D. Add a random document identifier

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: C.** `textContent` inserts plain text without interpreting it as HTML. The other choices do not correct the unsafe DOM operation.

</details>

**8.** An application passes user-provided filenames into a shell command. Which design most directly reduces command injection risk?

A. Use a direct library or API instead of invoking a shell  
B. Enable a longer session timeout  
C. Add an HttpOnly cookie  
D. Randomize database record identifiers

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: A.** Avoiding shell interpretation removes that command-construction boundary. Validate inputs and use least privilege as additional safeguards.

</details>

**Score guide:** 7–8: ready to advance; 5–6: review missed concepts; 0–4: revisit the instructional sections and repeat the lab.

## 17. Discussion challenge

A developer argues that HTTPS, MFA, and a WAF make server-side authorization checks unnecessary. Explain why each control addresses a different problem. Describe how you would validate access controls using fictional accounts and records in an authorized test environment.

<a id="13-study-from-the-treadmill"></a>

## 18. Study from the Treadmill 🏃

Walking, commuting, or taking a break from your desk? Reinforce today's lesson with these free Professor Messer Security+ SY0-701 videos. You do not need to follow along with a lab while watching.

| Video | Duration | Focus |
| --- | --- | --- |
| [SQL Injection — 2.3](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/sql-injection-sy0-701/) | 5:09 | How untrusted input changes database queries |
| [Cross-site Scripting — 2.3](https://www.youtube.com/watch?v=PKgw0CLZIhE) | 8:34 | Reflected and stored XSS and browser trust |
| [Application Attacks — 2.4](https://www.youtube.com/watch?v=yRSqIGjeb7s) | 11:48 | Injection, replay, privilege escalation, and CSRF |
| [Application Security — 4.1](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/application-security-sy0-701/) | — | Secure cookies, input validation, and application defenses |

**Study challenge:** Can you explain the difference between SQL injection, XSS, and CSRF without looking at your notes? For each, identify the trust boundary that fails and one appropriate mitigation.

The first three videos total 25 minutes and 31 seconds. The fourth is additional reinforcement.

### References and further reading

- [CompTIA Security+ certification](https://www.comptia.org/certifications/security)
- [OWASP Top Ten](https://owasp.org/www-project-top-ten/)
- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [OWASP Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
- [OWASP Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)

## 19. One-minute recap

- Keep untrusted data separate from interpreter instructions.
- Use parameterized queries, safe rendering, and server-side authorization.
- Protect sessions with rotation, expiration, revocation, and appropriate cookie settings.
- Prevent forged requests with anti-CSRF controls.
- Correlate alerts with evidence before claiming exploitation.

**Next:** Day 33 — continue Week 07.

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
