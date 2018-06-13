# On Behaviour Driven Development

## On Using Tools Separating Natural Language From Test Implementation
Many have been burned by test tools that encourage specifying a test in natural language that is translation to executable code, such as tools like FitNesse & Gherkin.
The thinking I've heard largely aligns with James Shore and his description of the [problems with acceptance testing](http://www.jamesshore.com/Blog/The-Problems-With-Acceptance-Testing.html).

For the record I'm actually OK with using tools like this.  The primary reason is I've rarely seen developers do a great job of mixing customer and developer jargon in the one artifact.
The theory here is that a customer specification mixed with code will likely detract from the quality of the specification, polluting it with developer speak (code & implementation concepts).

I completely agree that customers rarely exclusively write the specification, that translation from specification to code is overhead.
The point about changing these tests being more difficult is less valid present day as tools do exist to help you change the specification and translation layers.
