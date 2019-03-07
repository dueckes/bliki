# On Architecture Patterns

## Monoliths
A single artefact deployed as one component whose design and implementation is disorganised.
This pattern can eventuate for a numbers of reasons:
- Unintentionally: A modular design was intended, but internal architecture & design was loosely considered and enforced and/or implementors were poorly skilled. 
- Intentionally: A view that speed-to-market is hastened through little to no architecture and design consideration.  Often there is a plan put in place to perish the monolith an implement a pattern when time pressures permit.

This pattern typically leads to a component whose development and/or operations becomes so time consuming and complex that working on it becomes no longer economically viable.

Resources:
-[Monolithic System definition](https://en.wikipedia.org/wiki/Monolithic_system) 

## Modular Monoliths
A single artefact deployed as one component whose design and implementation is well organised.
This is also known as _Beautiful Monoliths_ and _Majestic Monoliths_.

Their internal design consists of highly decoupled parts with a view to achieving the following engineering qualities:
- Minimising the chance of change to one part effecting behaviours in others, reducing the chance of inadvertent error.
- Reducing cognitive load on developers by only requiring knowledge of the part being changed.
- Improving the developer experience by only requiring checking & verification of the part under change.
- Allowing a part to be deployed and scaled separately as required to address non-functional concerns.

A part usually:
- Encapsulates a domain.
- Is generated as a library within the single deployable artefact.
- Expresses dependencies on other parts as external dependencies/libraries.
- Isolates its storage from other parts, for example via a dedicated and separated database schema.
- Isolates its configuration from other parts.

Adding a dependency to another part is a significant design change and should never be as simple as importing a file or class - this is too easy to do without realising the design implications.
Consequently the following controls are usually put in place:
- A package management tool is used where each part is expressed as a package/library.
- Scoping through private, protected, public modifiers is taken very seriously, with the absolute minimum amount of parts made public.
- Static analysis tools are used to analyse and enforce dependencies between parts, such as [JDepend](https://github.com/clarkware/jdepend) and [JArchitect](https://www.jarchitect.com).

Their internal design is typically guided by Domain Driven Design theory.  For example, attention is paid to bounded contexts to ensure that a concept is not confused and polluted by multiple contexts.

As with most architecture patterns, it requires considerable engineering discipline to sustain.
When done poorly it is most at risk of devolving into a conventional Monolith.

Resources:
- [Modular Monoliths - Simon Brown presentation](https://www.youtube.com/watch?v=5OjqD-ow8GE)
- [The Majestic Monolith (Basecamp)](https://m.signalvnoise.com/the-majestic-monolith/)

## Service Orientated Architecture
Many artefacts deployed as separate physical components with no insinuation as to their quality - they may be organised or disorganised.
Critically, a set of services work together to make-up the system.  The boundaries and size of the services are arbitrary, but best guided by Domain Driven Design theory.

Because the architecture is distributed and the network inherently unreliable, fault tolerance becomes a strong consideration in the architecture.
Mechanisms may be necessary to better guarantee delivery, assure recovery or improve responsiveness to counter integration lag.

This pattern is largely unspoken nowadays as the term quickly became ambiguous and coupled to approaches such as WSDL based web services and tooling in the EAI/Middleware space.
Middleware was commonly abused with this pattern and given too much responsibility, leading to an architecture that breaks the rule of 'smart endpoints and dumb pipes'. 

Resources:
- [Service Orientated Architecture](https://en.wikipedia.org/wiki/Service-oriented_architecture)
- [Service Oriented Abiguity](https://martinfowler.com/bliki/ServiceOrientedAmbiguity.html)
- [Smart endpoints and dumb pipes](https://martinfowler.com/articles/microservices.html#SmartEndpointsAndDumbPipes)

## Microservices
Many small artefacts deployed as separate physical components with no insinuation as to their quality.

This pattern is similar to Service Orientated Architecture and suffers from the same distribution consequences, but is distinguished by a focus on the following benefits:
- Making things small enough to be quickly replaceable.
- Allowing each service to use a language and tools most suited to its problem space.
- Services are modifiable and deployable fast because their scope is heavily limited.
 
When done poorly it is most at risk of devolving into a Distributed Monolith or Microlith.
 
Resources:
- [Microservices](https://en.wikipedia.org/wiki/Microservices)
- [The Evolution Of Scalable Microservices](https://www.oreilly.com/ideas/the-evolution-of-scalable-microservices)

## Serverless
Many functions deployed as separate physical components with no insinuation as to their quality. 
This is also known as _Functions as a Service (FaaS)_.

The functions are most typically managed by a cloud provider, but may be managed by an operations team.  Either way the development team are ambivalent to the infrastructure (hence _Serverless_).

The benefits of this architecture are:
- Lower operational overheads as the infrastructure is often completely managed.
- Potential cost savings as cloud providers only bill when a function is active (consuming compute).

When cloud provider functions are used the following should be considered:
- Likely cloud provider lock-in.  This is particularly so for functions triggered by cloud provider infrastructure events, such as data arriving in a cloud provider file or data store.
- Language and version support is constrained by the provider.
- Limits apply on the functions such as execution time and number executing in parallel.
- Integration tests cannot be performed locally (disconnected from the network). 

This pattern is a realisation of the Nanoservices pattern that aims for extremely small sized artefacts and components.

Resources:
- [Serverless architectures](https://martinfowler.com/articles/serverless.html)
- [Nanoservices](https://www.infoq.com/news/2014/05/nano-services)
