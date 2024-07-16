# Types in BQN

It's well that typing array languages is difficult. A type checker for BQN would be next to useless
unless it can keep track of the shape of arrays. However, the presence of rank polymorphism as well
as primitives like Reshape makes this task highly nontrivial. Writing excessive type annotations to
help a type checker understand your code is undesirable, and doubly so in such a terse language. In
the general case, determining the most accurate type for an expression is equivalent to the halting
problem! (This can be demonstrated trivially using `•BQN`, but it can also surface in somewhat less
artificial cases.)
