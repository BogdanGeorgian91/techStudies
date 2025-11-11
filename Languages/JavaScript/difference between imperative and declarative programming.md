The key difference between imperative and declarative programming paradigms lies in *how* they instruct the computer to achieve a result:

- **Imperative programming** focuses on *how* to do things. It involves writing explicit step-by-step instructions (statements) that change the program’s state to achieve a desired outcome. You tell the computer exactly what to do and how to do it, often managing variables and control flow explicitly. For example, looping through a list to sort it by swapping elements is an imperative approach.

- **Declarative programming** focuses on *what* you want done, not how. It describes the desired result or goal without specifying the detailed control flow or state changes. The underlying system figures out the how. Declarative code is based on expressions that evaluate to values and often avoids mutable state. Examples include SQL queries or HTML markup, where you specify what you want, and the system handles the process.

### Summary of Differences

| Aspect               | Imperative Programming                     | Declarative Programming                      |
|----------------------|--------------------------------------------|---------------------------------------------|
| Focus                | How to perform tasks (step-by-step)        | What the desired outcome is                  |
| Code style           | Uses statements that change program state  | Uses expressions that evaluate to values     |
| State management     | Explicit and mutable                        | Implicit and often immutable                  |
| Control flow         | Explicitly defined by the programmer       | Implicit, handled by the language/framework  |
| Abstraction level    | Lower-level, more control                   | Higher-level, abstracts implementation details|
| Examples             | C, Java, Python (loops, conditionals)      | SQL, HTML, functional programming languages  |

### Metaphor
- Imperative: "Build the house by laying the foundation, erecting walls, installing utilities, etc." (detailed steps)
- Declarative: "I want a house with a fireplace and lakefront view; you decide how to build it"

In essence, imperative programming tells the computer *how* to do things, while declarative programming tells the computer *what* to do.
