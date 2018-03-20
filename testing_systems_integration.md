# Testing Systems Integration

There are a number of considerations when testing integration across systems:
* __Correctness Confidence__: To what extent does the test ensure correctness of the integration and overall functionality desired?
* __Reliability__: How often will the test be impacted by any fragility in the integrating system?
* __Isolation__: How can we ensure that a tests execution does not impact the execution of others so that they can execute reliably and ideally in parallel?
* __System Setup__: How difficult is it to setup and sustain the configuration and data required for the test, aligning the configuration and data dependencies across the systems?
* __Speed__: How quickly can the test give developers feedback?  Can developers get timely feedback and remain in-context?
* __Visual Inspection__: In situations where user interfaces are concerned, can I visually inspect the test and gain assurance of the overall user experience?
 
Many of these test concerns are broad and not unique to systems integration testing.

As with anything in software development, there are a number of approaches to testing integration across systems.
Let's look at these, and reflect on the considerations we listed above.

## Real Vs Virtualised System

Those integrating with a system may have a choice between relying on the real system or a simulation of the system.

### Real System

* __Correctness Confidence__: It can provide absolute confidence that the integration to a particular environment is correct, provided your test and its proof are sufficiently comprehensive.
* __Isolation__: Due to the shared state imposed by the real system, tests are more difficult to isolate.  To achieve isolation we may have to make data exclusive to a test or run the tests serially and ensure data is returned to its original state after modification.
* __System Setup__: The effort here is determined by the setup of the system to which you're integrating and its dependencies.  At best, you can automate any setup required.  At worst, frequent seeding needs to be done manually, requires coordination and needs to span many dependent systems.
* __Speed__: This often leads to the slowest integration tests.  However the speed will be proportional to that of the real systems interaction.
* __Visual Inspection__: This can be arduous as system setup must take place for each visual test scenario.

### System Simulation

Aka [virtualised system](https://en.wikipedia.org/wiki/Virtualization).

* __Correctness Confidence__: This is more difficult to achieve.  The real system must constantly prove it is consistent with the simulation.
* __Isolation__: Tests can be isolated with much less effort as some simulations allow interactions and their side-effects to be completely isolated from other interactions.  An example of this is [http_stub's sessions concept](https://github.com/MYOB-Technology/http_stub/wiki/Stub%20Sessions).
* __System Setup__: The setup of virtual systems is often extremely simple.  For example [http_stub allows consumers to simply activate scenarios](https://github.com/MYOB-Technology/http_stub/wiki/Scenarios) to setup the system.
* __Speed__: Virtual systems are usual simple state machines, trivial in complexity and are highly-performant.
* __Visual Inspection__: The setup for each visual test scenario can be simply achieved by priming the simulation.  For example, [http_stub provides a user interface allowing scenarios to be activated](https://github.com/MYOB-Technology/http_stub/wiki/Diagnostic-Pages#listing-the-scenarios).

## Producer Or Consumer Managed System

Those integrating with a system may have a choice between relying on a producer managed environment or self-managing an environment.
In both cases the same considerations apply to a real or simulated system.

### Producer Managed System

* __Correctness Confidence__: For absolute confidence, the producer must ensure that the software in their test environment is consistent to that which is in production.
* __Reliability__: This is dependant on the producers ability to keep the test environment available, making it less reliable.
* __Isolation__: This is the least isolated approach as the test environment may be used by many consumers, making test isolation more challenging.
* __Speed__: Test execution is impacted by other load on the producer system.  However, the speed will be proportional to the users experience of the system interaction.

## Self-Managed By Consumer

* __Correctness Confidence__: Consumers must ensure that their instance of the producer is up-to-date and consistent with what the producer has in production.
* __Reliability__: Tests are more reliable as the consumer is in complete control of the test infrastructure.
* __Isolation__: Tests are more isolated as they are not impacted by the use of other systems.
* __Speed__: Test speed is more stable as no other load will come into effect.
