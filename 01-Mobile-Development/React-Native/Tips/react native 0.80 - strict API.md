0.80 introduces the Strict TypeScript API: an opt-in type system that only includes what’s exported at the top-level of the react-native package:

The magic here is under the hood: the new types are generated directly from the actual React Native source code, not hand-written or community-maintained. That means they’re more accurate, better aligned with how the runtime behaves, and way less likely to drift over time.

To opt in, update your tsconfig.json like this:
  ![[IMG_6187.png]]

That one line tells TypeScript to resolve types from React Native’s new types_generated/ directory—an auto-generated set of types built directly from the source code.

If you’ve never had issues with React Native’s types, that’s kind of the point. The old system worked, but it was duct-taped together and prone to silent breakage. Opting in gives you a cleaner boundary and makes future upgrades safer.

As for [@types/react-native](https://qxeaom.clicks.mlsend.com/tl/c/eyJ2Ijoie1wiYVwiOjEwNzEwNTYsXCJsXCI6MTU3NDQ1MzQ2Mzg1OTg2OTM0LFwiclwiOjE1NzQ0NTM1OTg4MTE2MDIwMX0iLCJzIjoiZjkwYzViNGNjZDZmNzkwNyJ9), it’s still in use—but not for long. We predict the goal is to ship types with React Native itself, no DefinitelyTyped required. Strict mode is the first real step in that direction.