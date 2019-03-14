# On Domain Driven Design

## The Fallacy Of DDD Tooling
Q. Are there tools I can use to simplify the effort of implementing DDD?

A. Essentially, __No__.

DDD cannot be bottled in a library/package or an IDE.  It cannot be expressed in re-usable code.  It encompasses many things on many fronts.

_It is both a way of thinking and a way of doing._

It's theory spans analysis, architecture, design and implementation concerns.

## Analysis
- Domain Vision Statement
- Build & Refine Ubiquitous Language
- Forging Strong Relationships with Domain Experts
- Knowledge Crunching
- [Event Storming](https://www.eventstorming.com/)

### Identifying Domains Of Strategic Importance
- [Purpose alignment model](https://www.alaska.edu/files/pathways/WhitePaperonPurposeBasedAlignmentModel.pdf)
- [Domain strategic mapping](https://vimeo.com/189984496)
- [Wardley maps](https://realtimeboard.com/blog/wardley-maps-whiteboard-canvas)

### On Dealing With Complexity
- [Cynefin framework](https://en.wikipedia.org/wiki/Cynefin_framework)
- [Cynefin explained](https://www.youtube.com/watch?v=L5fnxahydXM)

## Architecture
- System Metaphor

### Identifying System Boundaries
- Context Maps, Bounded Context
- Organise Services & Systems:
  - By Domain
  - By Bounded Context

### Organisation Structure
- Team Collaboration Models: Shared Kernel, Customer/Supplier Teams, Conformist, Open Host Service, Separate Ways
- Theory of constraints - what parts of the architecture & implementation slow the organisation down, how should the technology respond to that?  [DDD and Theory of Constraints - Video](http://www.ustream.tv/recorded/102893159), [DDD and Theory of Constraints - Blog](http://ntcoding.co.uk/blog/2017/01/finding-service-boundaries-one-rule)

## Design

### Patterns
- Expressing the Domain Model
  - Domain Objects: Value Objects, Entities, Aggregates
  - Factories
  - Services
  - Specifications
  - Modules (packages organised by domain concept - known by Simon Brown as 'feature packages')
  - Accommodating integration:
    - Repositories
  - Accommodating large scale structure:
    - Responsibility Layers
    - Knowledge Level (originated in Martin Fowler's Analysis Patterns)
- For isolation/segregation of Domain & its behaviour:
  - Layered Architecture (Strict, Loose), Hexagonal Architecture, Ports & Adapters, Core & Shell
  - Anticorruption Layer
- Approaches to capturing data:
  - Classical (mutable current state)
  - Event Sourcing (strongly modelling events)
  
## Implementation
- Naming consistent with ubiquitous language!

### On Readability And Clarity Of Responsibility
- Intention revealing interfaces
- Closure of operations

### On Ensuring Correctness 
- Side effect-free functions
- Assertions

### On simplicity
- Standalone classes 
 