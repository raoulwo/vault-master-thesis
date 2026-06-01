---
author: Kanerva, P.
year: 2009
tags:
  - holographic-reduced-representation
  - hyperdimensional-computing
  - high-dimensional-vectors
pdf_link: "[[Hyperdimensional_Computing__An_Introduction_to_Computing_in_Distributed_Representation_with_High-Dimensional_Random_Vectors.pdf|Open PDF]]"
---
# Hyperdimensional Computing: An Introduction to Computing in Distributed Representation with High-Dimensional Random Vectors

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper provides an introduction to cognitive models that have emerged in the 1990s that depend on very high dimensionality and randomness.

## Related Work

%% Existing state-of-the-art research. %%

## Contributions

%% Research gap and novel contributions. %%

The paper is written as a tutorial essay to make concepts behind the cognitive models that depend on very high dimensionality and randomness more accessible.

### The von Neumann Architecture

The basic idea behind high dimensionality is to compute with large random patterns (high-dimensional random vectors) since they have subtle mathematical properties that are useful for computing.

The *arithmetic-logic unit* (ALU) is a component of a CPU that is used for basic math operations.
The paper deals with memory and ALU for large random patterns.

### An Engineering View of Computing

From an engineering perspective, computing is the *systemized and mechanized manipulation of patterns*. To transform patterns, computers have *circuits*. The *logic* of the circuit depends heavily on the underlying *representation* of the numbers (e.g. one's-complement, two's-complement).

Computers use *binary representation* almost exclusively. Representation must satisfy the condition of being *discriminate*. The bit patterns for different things must differ from one another.

*Dimensionality reduction* is a standard practice when processing high-dimensional data. It's however also possible that very high dimensionality facilitates processing. High dimensionality makes algorithms and circuits for high-precision arithmetic possible for example. This is because scalar mathematical values are actually represented using one-dimensional bit sequences.

### Properties of Neural Representation

#### Hyperdimensionality

High-dimensional spaces and vectors are called *hyperdimensional* when their dimensionality is >= 1000. *Hyperspace* is used for *hyperdimensional space* and *hypervector* for *hyperdimensional vector*.

The theme of the paper is that hyperspaces have subtle properties on which to base a *new kind of computing*.

#### Robustness

The neural architecture is tolerant of component failure due to redundant representation in which many patterns are considered equivalent. It's unlike standard binary representation where a single flipped bit means that the number is different.

#### Independence from Position: Holistic Representation

For maximum robustness (or most efficient use of redundancy) the information encoded into a representation should be distributed equally over all the components, meaning over the entire high-dimensional vector.

When bits fail, the information degrades in relation to the number of failing bits *irrespective of their position*.

#### Randomness

The system builds its model of the world from random patterns. That is, by starting with vectors drawn randomly from the hyperspace. The reasoning for this is as follows: If random origins can lead to compatible systems, the incompatibility of hardware ceases to be an issue. 

### Hyperdimensional Computer

#### Hyperdimensional Representation

The units used a computer computer with make up its *space of representations*. For example, a computer with a 32-bit ALU and 4 GB of memory can be thought of as having 32-bit binary vectors as representational space. They are the *building blocks* from which further representations are made.

10'000-dimensional binary vectors are great for demonstrating important properties of hyperdimensional representations. The representational space consists of $2^10000$ *points*.
The space can be thought of as the *corners of a 10'000-dimensional hyper-cube*.

Distances between the *points* in representational space can be measured in *Euclidean* or *Hamming* metric. For binary spaces Hamming distance being simpler to compute:

- It's the number of places at which two binary vectors differ.
- It's also the length of the shortest path from one corner point to the other along the edges of the hypercube. There are $2^k$ such shortest paths between two points $k$ bits apart.

For $k = 10000$, the Hamming distance is 10'000 bits. Note that the distance is often expressed as *relative* to the number of dimensions. So $10'000 bits / 10'000 = 1$.

An important property here is that while points are not concentrated or clustered in the representational space, *distances are highly concentrated half-way into the space* (roughly 5'000 bits)! This represents a binomial distribution with mean of 5'000 and standard deviation (STD) of 50. 

This means that a 600-bit wide "bulge" around the mean distance of 5'000 bits contains nearly all of the space. In other words, if we take two vectors at random and use them to represent meaningful entities, they differ in approximately 5'000 bits. If we then take a third vector at random, it will differ in approximately 5'000 bits from the first two and so on. Such vectors are called *unrelated*. Measured in standard deviations, the bulk of space, and the unrelated vectors, are 50 standard deviations away from any given vector.

This specific distribution of space is what makes hyperdimensional representation so robust. When meaningful entities are represented by 10'000-bit vectors, many of the bits can be changed (more than a third) through random errors and noise, and the resulting vector can still be identified with the correct one through it's smaller distance from the error-free vector with near certainty.

*Similarity* is the flip-side of *distance* when talking about patterns. Two patterns are similar when their distance is considerably lower than 0.5.

#### Hyperdimensional Memory

It is possible to build memory for storing 10'000-bit vectors that is addressed using 10'000-bit vectors, although $2^10000$ locations is far too many too be built (or needed).

#### Hyperdimensional Arithmetic

- *Weighting*. Multiplying each component of a vector with a scalar.
- *Comparison*. The resulting measure, a *number*, is often used as a weighting factor.
- *Addition*. To conform to distributional assumptions about representation, the arithmetic-sum-vector is *normalized* after component-wise addition. The resulting normalized sum is *similar* to the vectors being added together.
- *Subtraction*. Is performed by addition of the vectors' complement.
- *Multiplication*. This includes the *inner product*, the *outer product*, *weighting*, and *matrix multiplication*.

### Constructing a Cognitive Code

%% TODO: Continue here. %%

## Methodology

%% Architecture of the solution. %%

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

---

## Links

%% Links to related topics. %%