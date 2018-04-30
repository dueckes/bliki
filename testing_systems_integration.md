# Testing Systems Integration
There are a number of considerations when testing integration across systems:
* __Correctness Confidence__: To what extent does the test ensure correctness of the integration and overall functionality desired?
* __Reliability__: How often will the test be impacted by any fragility in the integrating system?
* __Independence__: How can we ensure that a test's execution does not impact the execution of others so that they can execute reliably and ideally in parallel?
* __System Setup__: How difficult is it to setup and sustain the configuration and data required for the test, aligning the configuration and data dependencies across the systems?
* __Speed__: How quickly can the test give developers feedback?  Can developers get timely feedback and remain in-context?
* __Visual Inspection__: In situations where user interfaces are concerned, can I visually inspect the test and gain assurance of the overall user experience?
 
Many of these test concerns are broad and not unique to systems integration testing.

As with anything in software development, there are a number of approaches to testing integration across systems.
I'll look at these in detail below and reflect on the considerations listed above.

However, before we get there I'd like to make one thing perfectly clear: _there is no one size fits all_, no approach that one should always follow with others cast aside and never considered again.
Having a general preference or ideal approach is fine, but the approach we take at any time will be based on the context of that integration - the nature of the system we're integrating with and its complexities.
This context will shape the pro's and con's of the approaches and should drive the decision making.

## Real Vs Virtualised System
Those integrating with a system may have a choice between relying on the real system or a simulation of the system.

### Real System
* __Correctness Confidence__: It can provide absolute confidence that the integration to a particular environment is correct, provided your test and its proof are sufficiently comprehensive.
* __Independence__: Due to the shared state imposed by the real system, it is more difficult for the test's to be independent.  To achieve independence we may have to make data exclusive to a test or run the test's serially and ensure data is returned to its original state after modification.
* __System Setup__: The effort here is determined by the setup of the system to which you're integrating and its dependencies.  At best, you can automate any setup required.  At worst, frequent seeding needs to be done manually, requires coordination and needs to span many dependent systems.
* __Speed__: This often leads to the slowest integration test's.  However the speed will be proportional to that of the real systems interaction.
* __Visual Inspection__: This can be arduous as system setup must take place for each visual test scenario.

### System Simulation
Aka [virtualised system](https://en.wikipedia.org/wiki/Virtualization). 

* __Correctness Confidence__: This is more difficult to achieve as the real system and simulation must constantly be proven to be consistent whenever either change.  Tooling that [generates simulations from the real systems artifacts](https://github.com/swagger-api/swagger-codegen/wiki/server-stub-generator-howto) can significantly help here.
* __Independence__: Tests can be independent with much less effort as simulations can [isolate any side-effects caused by a test](https://github.com/MYOB-Technology/http_stub/wiki/Stub%20Sessions).
* __System Setup__: The setup of virtual system is often very simple.  Tools can afford achieving this [setup simply and dynamically](https://github.com/MYOB-Technology/http_stub/wiki/Scenarios).
* __Speed__: Virtual systems are usually highly performant as they are of trivial complexity, often outperforming real systems.  There performance is also largely O(1), regardless of the integration being performed.
* __Visual Inspection__: The setup for each visual test scenario is achieved by priming the simulation, some virtually systems making this [trivially simple](https://github.com/MYOB-Technology/http_stub/wiki/Diagnostic-Pages#listing-the-scenarios).

## Producer Or Consumer Managed System
Those integrating with a system may have a choice between relying on a producer managed environment or self-managing an environment.
In both cases the same considerations apply to a real or simulated system.

### Producer Managed System
* __Correctness Confidence__: For absolute confidence, the producer must ensure that the software in their test environment is consistent to that which is in production.
* __Reliability__: This approach risks being less reliable and is dependant on the producers ability to keep the test environment available.
* __Independence__: Other consumers of the producer system can impact the results of your test's, and may need to be mitigated by scheduling or regular system setup exercises.
* __Speed__: Speed can be reduced by load from other consumers of the producer system.  However, the speed will be similar or identical to the real performance depending on the production likeness of the environment.  This can serve to help surface performance issues sooner.

### Self-Managed By Consumer
* __Correctness Confidence__: Confidence is again proportional to the self-managed environments consistency with the production environment, but the burden here now shifts to the consumer.  They must spend additional effort to ensure ongoing consistency.
* __Reliability__: Greater reliability is achieved as the consumer is in control of the infrastructure involved in the test.
* __Independence__: Greater independence is achieved as other no other consumers can impact the state of the system.
* __Speed__: Speed stabilises as no other consumers will use the system.
