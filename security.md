# On Continuous Security Validation
Continuous delivery promotes the ongoing validation and release of software one change at a time to mitigate the risk of large change that occurs in waterfall methods.
Unfortunately these pipelines often exclude security validation - _and the truth is they shouldn't_.
For mind, the industry has largely missed this step for a few reasons:
1. Inadequate tooling: A lack of automated tools to simplify the effort involved.
2. Inadequate knowledge: Security concepts are not taught as frequently and as in-depth as classical development constructs, principles and practices.
3. Perceived low attack threat: An assumption that software is so vast (as it is 'eating the world') and that the number of hackers was so small that statistically it will 'never happen to me'.

An early adopter shift is in motion at the moment.  The forces effecting all three of these reasons are changing.
Any change going into production must and should go through a minimum set of security verification - with that set determined by the application's risk profile.

# OWASP ASVS 3.0.1

## What is ASVS?
The OWASP Application Security Verification Standard intends to help individuals, teams and organisations guide the security of their software assets.
It was first introduced in 2008 and has been produced via a community of security experts.

At its heart it defines a set of security requirements for applications builders to consider.
It categorises these requirements into a level (1 to 3).  These categories are effectively a _security threat level_ and recognise that the priority of these requirements differs depending on the profile and sensitivity of an application.
Level 1 is for applications warranting minimum security, whereas level 3 is for application warranting the utmost security.

## Security verification levels

__Level 1 - 'Opportunistic'__: applies to any application.
__Level 2 - 'Standard'__: applies to applications handling [PII](https://en.wikipedia.org/wiki/Personally_identifiable_information), PCI (Payment Card Industry) data that can lead to small amounts of financial loss, or non-critical secrets (e.g intellectual property).
__Level 3 - 'Advanced'__: applies to applications that can lead to the loss of critical secrets (e.g. intellectual property), large amounts of financial data or can threaten health.

## The requirements in brief

#### Architecture, design and threat modelling
__Purpose:__ Ensure the architecture of the application and threats inherent to this are known. 

##### Level 1
* Application components and their purpose are known.

##### Level 2
* Internal and external dependencies, such as libraries or external API's, are known.
* High level architecture is known.

##### Level 3
* Apply [STRIDE threat modelling](https://dzone.com/articles/stride-threat-model) identifying what can go wrong and possible countermeasures ([example](https://www.securityweek.com/threat-modeling-internet-things-part-3-real-world-example)).
* The business and security function of components and dependencies (internal and external) are known.  These are also known to have no vulnerabilities.
* Components are separated and restrict access through mechanisms such as firewalls or security groups.
* All security components and dependencies reuse a common verified asset and are not implemented bespoke.
* Client side code does not contain data such as secrets or business logic representing IP.

#### Authentication
__Purpose:__ Ensure all credentials are handled securely and that interactions are with trusted parties.

##### Level 1
* Secure pages are only accessible when authenticated.  Includes administration interfaces.
* Server endpoints are only accessible with appropriate authentication.
* Authentication failures do not reveal valid credentials.
* Forms with credentials are never pre-filled.  Browser autocomplete and password managers are permittable exceptions.
* Passwords are encouraged to be difficult to discover (complex & long).  Common passwords are disallowed.
* Password changes require old password and password confirmation.
* Account recovery features (e.g. forgot password) do not allow an accounts to be hijacked or mined (accounts discovered).  They should not reveal a password or send in clear text.  They should use a one-time time based token and expire if not used.
* All communications between components is encrypted.
* No common passwords are in use to secure system components.
* Anti-automation is in-place to inhibit brute-force attacks and password lock-outs.
* Secret questions (e.g. mother's maiden name) do not violate privacy laws and a difficult to discover.

##### Level 2
* There is an audit trail of authentication decisions.  Credentials are not logged.
* Passwords are persisted with a one-way hash that's incredibly difficult to reverse engineer.
* Credentials for external components are encrypted at rest in a secure location.
* Accounts have both soft lock-outs (temporary) and hard lock-outs (durable, requiring verification).
* Password rules can disallow some number of previous passwords.
* Re-authentication or two-factor is in-place for high value transactions.  ATN
* Users can choose to authenticate using more than just username and password (e.g. using two-factor).

##### Level 3
* Authentication requests have the same average response time, regardless of success or failure.
* Secrets and keys are not in source code.

#### Session management
__Purpose:__ Ensure sessions are secured and invalidated appropriately.

##### Level 1
* Session management uses a common verified asset (e.g. library).
* Sessions are not usable post log-out.
* Sessions timeout due to inactivity.
* Users can logout from all client pages.
* Session ID's are never disclosed publicly.
* Session ID's are difficult to discover: unique, random and long.
* Every login and re-login generates a unique session.
* Session ID's in cookies are secure - paths are as restrictive as possible, `HttpOnly` and `secure` attributes are set appropriately.
* Concurrent sessions per user are limited.
* User can view a list of active sessions and terminate any session.  ATN
* User is prompted with option to terminate other active sessions on password change.  ATN

#### Level 2
* Session timeout periods are configurable.
* Sessions ID's cannot be forced, they must be generated by the application.

#### Access control
__Purpose:__ Ensure only permitted users can access and modify data.

##### Level 1
* [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) holds.  Any resource, UI or API is only accessible by permitted users.  [Spoofing](https://en.wikipedia.org/wiki/Spoofing_attack) and [elevation of privilege](https://en.wikipedia.org/wiki/Privilege_escalation) attacks are mitigated.  Manipulating a request (e.g. headers, parameters) does not give access to unpermitted resources.
* Directory browsing is disabled unless absolutely required.  Metadata files are always inaccessible (e.g. `.git`).
* Denying access does not compromise security.
* [Cross-site request forgery](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) protection is in-place.

##### Level 2
* State governing access should not be modifiable, unless authorised.
* All access decisions are logged, including failures.
* Rate limiting is in-place to reasonably limit requests and modification of access rights by a user.
* Additional authorisation is required for sensitive operations, via step-up or via separate authentication and authorisation systems for those operations that are most sensitive.

##### Level 3
* Authorisation assets are shared (e.g components, libraries).  Systems do not use bespoke authorisation technology.  ATN

#### Malicious input handling
__Purpose:__ Ensure inputs and data are appropriate, not imposing any security threats, before use.

##### Level 1
* Buffer overflows are not possible.
* Input is validated server side.  Validation failures are logged.
* [SQL injection](https://www.owasp.org/index.php/SQL_Injection), [LDAP injection](https://www.owasp.org/index.php/LDAP_injection) and [OS Command injection](https://www.owasp.org/index.php/Command_Injection) are prevented.
* [File inclusion](https://en.wikipedia.org/wiki/File_inclusion_vulnerability), rendering arbitrary files remotely or locally, is not possible.
* [XML attacks](https://www.owasp.org/index.php/Testing_for_XML_Injection_(OTG-INPVAL-008)) are validated against, ensuring document integrity and preventing external entity attacks. 
* Data included in HTML content is escaped, preventing [cross-site scripting](https://en.wikipedia.org/wiki/Cross-site_scripting).
* Data inputted as HTML is sanitised and escaped.

##### Level 2
* Automatic [mass assign](https://www.owasp.org/index.php/Mass_Assignment_Cheat_Sheet) via white-lists or black-lists attributes.
* Sensitive requests parameters (e.g `role`, `accountBalance`) are not open to manipulation.
* Input is validated client side.
* All sources of input are validated, with white-listed validation preferred over black-listed.
* The size and pattern of structured data is validated, via a schema if possible.
* The size and characters of unstructured data is validated and escaped.
* DOM data is shared using _.innerText_ and _.val_.
* Use `JSON.parse()`.  Avoid using `eval()` where possible. 
* Verify that sensitive data is cleared from the browser at session end.

##### Level 3
* The same validation logic is used for each data type.

#### Cryptography at rest
__Purpose:__ Ensure cryptography is used securely.

##### Level 1
* Use of cryptography fails securely and does not leak decryption information.  A [padding oracle attack](https://en.wikipedia.org/wiki/Padding_oracle_attack) is an example of leakage.
* Cryptographic algorithms are approved by [FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) or similar.
 
##### Level 2
* Random generators in use are recommended by cryptographic libraries.
* A key management policy is in-place and enforced.
* [PII](https://en.wikipedia.org/wiki/Personally_identifiable_information) is encrypted at rest and secure in-transit.
* Sensitive data is removed from memory when no longer required.
* Key and passwords are replaceable.

##### Level 3
* Cryptography library use is as per recommended good practice.
* Isolate cryptographic services and consider the use of a [HSM](https://en.wikipedia.org/wiki/Hardware_security_module).
* Random numbers are created with appropriate entropy. 

#### Error handling and logging
__Purpose:__ Log only what is needed and secure logs appropriately.

##### Level 1
* Error messages do not assist attackers or expose personal information.
* Audit logs provide evidence of key transactions (non-repudiation).
* Timing in logs is accurate via time synchronisation.

##### Level 2
* Error handling prevents access.
* The results of security questions are logged, both success and failure.
* Logs include data that offers the timeline of an event.
* Untrusted event data will not be executed as code when viewed, ala [XSS](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)).
* Security logs cannot be altered and limit access via authorisation controls.
* Logs do not contain sensitive data or any data that can assist an attacker.

##### Level 3
* Log content is correctly encoded and not manipulable via [log injection](https://www.owasp.org/index.php/Log_Injection).
* Logs from trusted vs untrusted sources are distinguishable.
* Security logs use integrity checks to prevent log tampering. 
* Logs are stored in a different partition and are rotated.
  
#### Data protection
__Purpose:__  Ensure data is appropriately secured in-transit and at rest.

##### Level 1
* Sensitive form fields are not cached client-side, including autocomplete's.
* Sensitive data is never sent clear text via HTTP (e.g. as `GET` parameters).
* Recommended anti-caching headers are in use.

##### Level 2
* Sensitive data cached server-side is secured and removed when no longer needed.
* Requests have a minimal number of parts (headers, parameters, cookies, etc).
* Access to sensitive data is logged.
* Sensitive data is removed from memory when no longer required. 

##### Level 3
* All sensitive data is identified and a policy secures this data.
* All sensitive data has a removal method in-place when it is no longer required.
* Detect and alert when data harvesting is taking place. 

#### Communications
__Purpose:__ Ensure all communications are secure.

##### Level 1
* Certificates are valid and issued by trusted [CA's](https://en.wikipedia.org/wiki/Certificate_authority).
* [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) is exclusively in-use with no fallback.
* [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) settings are current best practice.
* HTTP Strict Transport Security headers are on all requests and for all subdomains.
* Enable [perfect forward secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) to mitigate recording of traffic.
* Strong algorithms, ciphers, and protocols are used, through the entire certificate hierarchy.
* Certificate revocation is enabled and configured.

##### Level 2
* Connections to external systems are authenticated.
* [TLS certificate public key pinning (HPKP)](https://en.wikipedia.org/wiki/HTTP_Public_Key_Pinning) is in-place with backup keys or an alternative such as [certificate transparency enforcement](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Expect-CT).

##### Level 3
* The [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) version in-use is secure and appropriate.
* [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) connection failures are logged.
* Certificate paths are built using [trust anchors](https://en.wikipedia.org/wiki/Trust_anchor) and revocation information.

#### HTTP security configuration
__Purpose:__ Ensure applications follow HTTP security best practice.

##### Level 1
* Request methods accepted are limited.  Unused methods are blocked.
* Responses contain a `Content-Type` header using a safe character set (e.g. UTF-8, ISO 8859-1).
* Responses contain appropriate [`X-Content-Type-Options`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options), [`Content-Disposition`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition) and [`X-XSS-Protection`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection) headers.
* Responses do not reveal system component information (such languages, tools, versions in use).
* A content security policy mitigates DOM, XSS, JSON and Javascript injection.

##### Level 2
* Authentication headers are validated.
* An appropriate [`X-FRAME-OPTIONS` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) is in use with x-frames.

#### Malicious controls
__Purpose:__ The potential for malicious activity is limited, and any impacts are limited.

##### Level 3
* The application is [sandboxed](https://en.wikipedia.org/wiki/Sandbox_(computer_security)), preventing access to other components and applications.
* Source code and libraries in use do not contain [back doors](https://en.wikipedia.org/wiki/Backdoor_(computing)), [easter eggs](https://en.wikipedia.org/wiki/Easter_egg_(media)) or logic flaws in sensitive operations.

#### Business logic
__Purpose:__ Business logic is appropriate and detects and prevent attacks.

##### Level 2
* Business logic is only processable in an intentional sequential order with reasonable time-limits between steps.
* Business transactions are rate limited on a per user basis.  Alerting is in-place to inform of abuse.
 
#### File and resources
__Purpose:__ Untrusted file content is handled securely.

##### Level 1
* URL redirects are whitelisted or clearly show a warning.
* File data uploaded is not used in file I/O commands.
* File uploaded are of an expected type and appropriately scanned for viruses.
* Untrusted or uploaded data is not used in reflection, class loading or directly executed. 
* Untrusted data is not used to determine origins used for [Cross Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).
* Do not use technologies not supported by W3C standards (e.g. Flash, Silverlight). 

##### Level 2
* Untrusted files are stored outside the webroot and have appropriate permissions.
* Web servers deny access to resources outside of the application.

#### Mobile
__Purpose:__ Data is stored and transmitted security to mobile devices.

##### Level 1
* Never use device identifiers as authentication tokens.
* Sensitive data is not stored unencrypted or unprotected on device.
* Secrets (e.g. API or authentication tokens) are dynamically generated for mobile applications.
* Ensure memory is organised randomly (OS's typically implement [ASLR](https://en.wikipedia.org/wiki/Address_space_layout_randomization)).
* Sensitive activities, intents or content providers cannot be exploited by other mobile applications.
* Inputs to exported activities, intents or content providers are validated.

##### Level 2
* Sensitive information is not leaked (e.g. via screenshots or logs).  It is destroyed post-use.
* The application uses the minimal possible permissions to perform its functionality.
 
##### Level 3
* Debugging of the application is difficult and deterred.

#### Web services
__Purpose:__ Ensure web API's have appropriate security controls. 

##### Level 1
* Client and server encoding is consistent.
* Server administration functions are secured, only accessible by administrators.
* XML and JSON schema is used as part of request validation.  Inputs are size limited.
* An appropriate [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) version is in use.
* Session authentication and authorization measures are secure.
* Avoid static API keys.
* At least one [cross-site request forgery](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) countermeasure is in-place.  Potential options: [origin header verification](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#Checking_the_Origin_Header), [referer header verification](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#Checking_the_Referer_Header), [the double submit cookie pattern](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#Double_Submit_Cookie), CSRF [nonces](https://en.wikipedia.org/wiki/Cryptographic_nonce).

##### Level 2
* Validate the `Content-Type` header.
* Sign the content of messages, for example via [JSON Web Signing](https://ordina-jworks.github.io/security/2016/03/12/Digitally-signing-your-JSON-documents.html).
* Alternative access to data is not possible.
 
#### Configuration
__Purpose:__ Ensure configuration does not effect the security of components.

##### Level 1
* Components and libraries are configured with best security practice and versions are up-to-date.

##### Level 2
* Communications between components is encrypted and authenticated, following the [principle of least privilege]([least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege)).
* Applications are isolated, limiting the impacts of a breech.
* Application builds and deployments are preformed following good security practice.

##### Level 3
* Administrators can verify configuration to ensure its authenticity.
* All components are signed.
* All 3rd party dependency are from trusted sources.
* Builds have security flags enabled (e.g. [DEP](https://en.wikipedia.org/wiki/Executable_space_protection), [ASLR](https://en.wikipedia.org/wiki/Address_space_layout_randomization)).
* All assets are sourced from the application, not via content providers/[CDN's](https://en.wikipedia.org/wiki/Content_delivery_network). 
