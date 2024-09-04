# Number types

At runtime, BQN has a unified number type containing non-NaN double-precision floating point numbers.
The specification technically allows for other implementations, but in practice floats are the only
number type that satisfy the constraints. Integers and natural numbers are then subsets of this general
number type. (CBQN squeezes integer arrays as a memory saving optimization, but this is largely 
transparent and does not change their behavior.)

Certain operations are defined specifically on integers, such as `take ↑` and `drop ↓`, and error on 
non-integral numbers. Axes are represented by naturals. Most arithmetic operates on general numbers.
Some operations extend their domain with special numeric values: `rank ⎉` operates on the integers 
extended to include `infinity ∞`, and `group ⊔` operates on naturals extended to include `¯1`.

To model these relationships, we introduce subtyping (for numbers), singleton types, and union types.

In a nutshell, subtyping allows for the $x: A$ to have multiple valid assignments for $A$ rather than just one. For example, if $A \le B$ ($A$ is a subtype of $B$), then whenever $x: A$, $x: B$ is also valid.

Borrowing the concept of a "numeric tower" from contemporary dynamic programming languages, 
we define the following subtyping relationships: 

$$\vdash \text{Nat} \le \text{Int},\ \text{Int} \le \text{Number}$$

where $\text{Nat}$ is the type of non-negative integers, $\text{Int}$ is the type of integers, and $\text{Number}$ is the type of general numbers (i.e. extended real numbers).

Furthermore, we define singleton types $\text{Literal}(x)$ for each of these classes of number, such as $\text{Literal}(¯0.5)$:

$$x: \text{Nat} \vdash \text{Literal}(x) \le \text{Nat}$$

$$x: \text{Int} \vdash \text{Literal}(x) \le \text{Int}$$

$$x: \text{Number} \vdash \text{Literal}(x) \le \text{Number}$$

Note that $\text{Literal}$ as described is a $\Pi$ type. For implementation purposes, it may be necessary to restrict its domain to statically known constant values.

Finally, we define unions of types:

$$A, B\ \text{types} \vdash A \le A \cup B = B \cup A$$

Unions follow their set-theoretic properties, omitted here.

This framework allows us to represent model the number types used in a vast majority of BQN primitives. For ergonomics, we may also want primitives to have overloaded definitions. For example, addition is closed in its domains, and therefore:

$$\begin{align*}
+_{simple} &: \text{Nat} \to \text{Nat} \to \text{Nat} \\
&\cap \text{Int} \to \text{Int} \to \text{Int} \\
&\cap \text{Number} \to \text{Number} \to \text{Number}
\end{align*}$$

This requires the use of intersection types, which will be discussed in a separate document about blocks.
