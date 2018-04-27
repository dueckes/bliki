# On Continuous Security Validation
Continuous delivery promotes the ongoing validation and release of software one change at a time to mitigate the risk of large change that occurs in waterfall methods.
Unfortunately these pipelines often exclude security validation - _and the truth is they shouldn't_.
For mind, the industry has largely missed this step for a few reasons:
1. Inadequate tooling: A lack of automated tools to simplify the effort involved.
2. Inadequate knowledge: Security concepts are not taught as frequently and as in-depth as classical development constructs, principles and practices.
3. Perceived low attack threat: An assumption that software is so vast (as it is 'eating the world') and that the number of hackers was so small that statistically it will 'never happen to me'.

An early adopter shift is in motion at the moment.  The forces effecting all three of these reasons are changing.
Any change going into production must and should go through a minimum set of security verification - with that set determined by the application's risk profile.

# OWASP ASVS 3.0.1 Distilled

## What is ASVS?
The OWASP Application Security Verification Standard intends to help individuals, teams and organisations guide the security of their software assets.
It was first introduced in 2008 and has been produced via a community of security experts.

At its heart it defines a set of security requirements for applications builders to consider.
It categorises these requirements into a level (1 to 3).  These categories are effectively a _security threat level_ and recognise that the priority of these requirements differs depending on the profile and sensitivity of an application.
Level 1 is for applications warranting minimum security, whereas level 3 is for application warranting the utmost security.

## Security verification levels

__Level 1 - 'Opportunistic'__: applies to any application.
__Level 2 - 'Standard'__: applies to applications handling PII (identifies people), PCI data (credit card data) that can lead to small amounts of financial loss, or non-critical secrets (e.g intellectual property).
__Level 3 - 'Advanced'__: applies to applications that can lead to the loss of critical secrets (e.g. intellectual property), large amounts of financial data or can threaten health.

### Requirements

#### Architecture, design and threat modelling
__Purpose:__ Ensure the architecture of the application and threats inherent to this are known. 

##### Level 1
* Application components and their purpose are known.

##### Level 2
* Internal and external dependencies, such as libraries or external API's, are known.
* High level architecture is known.

##### Level 3
* The business and security function of components and dependencies (internal and external) are known.  These are also known to have no vulnerabilities.
* Components are separated and restrict access through mechanisms such as firewalls or security groups.
* All security components and dependencies reuse a common verified asset and are not implemented bespoke.
* Client side code does not contain data such as secrets or business logic representing IP.
* Apply [STRIDE threat modelling](https://dzone.com/articles/stride-threat-model) identifying what can go wrong and what countermeasures are possible ([example](https://www.securityweek.com/threat-modeling-internet-things-part-3-real-world-example)).

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
* Credentials for external component are encrypted at rest in a secure location.
* Accounts have both soft lock-outs (temporary) and hard lock-outs (durable, requiring additional validation).
* Password rules can disallow some number of previous passwords.
* Re-authentication or two-factor is in-place for high value transactions.  ATN
* Users can choose to authenticate using more than just username and password (e.g. using two-factor).

##### Level 3
* Authentication requests have the same average response time, regardless of success or failure.
* Secrets and keys are not in source code.

#### Session management
__Purpose:__ Ensure sessions are secured and invalidated appropriately.

##### Level 1
* Session management uses a common verified asset.
* Sessions are not usable post log-out.
* Sessions timeout due to inactivity.
* Users can logout from all client pages.
* Session ID's are never disclosed publicly.
* Session ID's are difficult to discover: unique, random and long.
* Every login and re-login generates a unique session.
* Session ID's in cookies are secure - paths are as restrictive as possible, 'HttpOnly' and 'secure' attributes are set appropriately.
* Concurrent sessions per user are limited.
* User can view a list of active sessions and terminate any session.  ATN
* User is prompted with option to terminate other active sessions on password change.  ATN

#### Level 2
* Session timeout periods are configurable.
* Sessions ID's must be generated by the application.

#### Access control
__Purpose:__ Ensure only permitted users can access and modify data.

##### Level 1
* Principle of least privilege holds.  Access is only given to permitted users.  [Spoofing](https://en.wikipedia.org/wiki/Spoofing_attack) and [elevation of privilege](https://en.wikipedia.org/wiki/Privilege_escalation) attacks are mitigated.  Manipulating a request (e.g. headers, parameters) does not give access to unpermitted resources.
* Directory browsing is disabled unless required.  Metadata files are inaccessible (e.g. .git).
* 

#### Malicious input handling
#### Cryptography at rest
#### Error handling and logging
#### Data protection
#### Communications
#### HTTP security configuration
#### Malicious controls
#### Business logic
#### File and resources
#### Mobile
#### Web services
#### Configuration
