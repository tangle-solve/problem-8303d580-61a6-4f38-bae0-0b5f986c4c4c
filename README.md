<!-- solve-problem:8303d580-61a6-4f38-bae0-0b5f986c4c4c -->
# Deterministic polynomial kernel for Odd Cycle Transversal

A [Tangle Solve problem](https://solve.tangle.tools/problems/8303d580-61a6-4f38-bae0-0b5f986c4c4c).

## What is the problem?

**Odd Cycle Transversal (OCT)**: given a graph G and integer k, decide whether there is a set S of at most k vertices whose deletion makes G bipartite (equivalently, S intersects every odd cycle). The parameter is k.

**Kernelization**: a polynomial-time algorithm mapping (G, k) to an equivalent instance (G′, k′) with |G′| + k′ ≤ f(k). If f is polynomial, this is a polynomial kernel.

**The open problem**: construct a *deterministic* polynomial-time algorithm producing a polynomial-size kernel for OCT. The known polynomial kernel (Kratsch–Wahlström) is randomized: it uses a matroid based on linear representations over finite fields and the Schwartz–Zippel lemma, and it succeeds with high probability but may output a false instance with small probability. Removing this randomness while keeping both polynomial time and polynomial size is open in general. Note: the problem is in FPT deterministically (e.g., iterative compression, Reed–Manning–Smith; Cygan et al.'s deterministic FPT algorithm); the obstacle is purely kernelization size.

A subtlety: the goal is not merely derandomizing the *decision* algorithm (done), but the *compression* — deterministic polynomial-size preprocessing. Also note OCT is "bipartization"; edge variant (Edge Bipartization) has similar status.

## Why does it matter?

Solving it would resolve one of the flagship open problems in kernelization (it is on the kernelization open-problem lists, e.g., in the Dell–Kratsch and later FPT/kernelization problem lists), and would likely yield a general technique for derandomizing algebraic kernels (matroid-based compression also gives kernels for Multiway Cut, Vertex Cover above LP, etc.). Practically, kernelization is the formal foundation of preprocessing for NP-hard problems; deterministic kernels guarantee reproducible, failure-free preprocessing pipelines. Progress would also clarify whether the randomization here is inherent or an artifact of the technique.

## What's already known?

- **Kratsch & Wahlström (2012)**: randomized polynomial kernel for OCT via representing a certain matroid (a "transversal-type" matroid built from random vectors) over GF(2^q) and using representative sets / matroid tools; failure probability 1/2, boosted by repetition. The matroid rank depends on the field size, which is where randomness enters.
- **Deterministic kernels exist for special graph classes**: e.g., OCT on planar graphs and H-minor-free graphs has polynomial (even linear/constant-factor) kernels via region/decomposition techniques.
- **Lower bounds**: under NP ⊆ coNP/poly, OCT admits no kernel of size O(k^{2−ε}) (Dell–van Melkebeek–Wahlström, via a reduction), so polynomial size is roughly tight in exponent but a deterministic O(k^c) kernel of any constant c is fine.
- **Related derandomization results**: deterministic representative-set computations exist for *linear* matroids given by independent-set oracles or representations; the obstacle is that the OCT kernel needs a *linear representation of a matroid that is only implicitly/randomly defined* (constructing deterministic representations of the relevant matroid class — related to deterministic construction of certain matroid representations — is open; see also derandomization questions raised by Marx, and by Lokshtanov et al. on "deterministic representative sets"). Some partial derandomizations exist for restricted parameterizations (verify current status).
- **FPT algorithms**: deterministic O(3^k · poly) via iterative compression; randomized and deterministic improvements in Cygan et al. (2015 book, Ch. on iterative compression).

The randomized kernel is not known to be derandomizable by standard tools (limited independence fails because the matroid representation itself must be over a field of exponential size in some sense).

## What would count as solving it?

Any of the following would resolve the problem:

1. **Construction**: a deterministic polynomial-time algorithm mapping any OCT instance (G, k) to an equivalent instance of size k^O(1).
2. **Reduction-rule approach**: a set of provably safe reduction rules yielding the same bound.
3. **Partial progress that counts**: a deterministic kernel for OCT on any significantly broader graph class than H-minor-free; a deterministic kernel of size k^O(log k); or a derandomization of a well-defined subclass of the Kratsch–Wahlström matroid machinery.
4. **Negative direction**: a formal barrier, e.g., showing deterministic polynomial kernelization of OCT would imply deterministic constructions of certain matroid representations believed hard, or a kernel lower bound under a stronger assumption.

A referee would accept: full proof of correctness, polynomial running time, and explicit size bound, in a self-contained writeup.

## How to start

1. Read Kratsch & Wahlström (2012) closely — understand exactly where randomness is needed: the representation of the matroid built from "annotated" vertex subsets via random field elements, and the field-size vs. failure-probability tradeoff.
2. Read the deterministic representative-sets literature (Fomin–Lokshtanov–Saurabh; Marx's representative sets for matroids) — check whether the obstacle is really matroid representation or the set-family structure.
3. Try small cases: can you deterministically represent the relevant matroid when k is small, or for bounded-treewidth graphs? Try replacing Schwartz–Zippel with explicit low-degree testing (e.g., via k-perfect hash families or arithmetic-derandomization à la deterministic identity testing for restricted circuit classes).
4. Main obstacle: no known deterministic construction of exponentially large linear representations of the needed matroids; attacking explicit construction of such representations (a derandomization question in its own right) is the natural — and hard — route.

## Key references

- S. Kratsch, M. Wahlström. *Compression via Matroids: A Randomized Polynomial Kernel for Odd Cycle Transversal*. SODA 2012 / TOCT 4(4), 2012. https://arxiv.org/abs/1107.1008 — gives the randomized polynomial kernel for OCT using linear matroid representation and representative sets; this is the paper to derandomize.
- M. Cygan, F. V. Fomin, Ł. Kowalik, D. Lokshtanov, D. Marx, M. Pilipczuk, M. Pilipczuk, S. Saurabh. *Parameterized Algorithms*. Springer, 2015. — textbook covering iterative compression for OCT, kernelization, and matroid-based representative sets; see the matroid chapter.
- B. Reed, K. Smith, A. Vetta. *Finding Odd Cycle Transversals*. Oper. Res. Lett. 32(4), 2004. — the original iterative-compression FPT algorithm for OCT.
- H. Dell, D. van Melkebeek, M. Wahlström (or the relevant kernelization lower-bound papers — verify authorship for the OCT lower bound) — conditional kernel size lower bounds near k^2 for OCT. (verify)
- D. Marx. *A parameterized view on matroid optimization*. FOCS 2009. — representative sets for linear matroids, the underlying tool of the kernel.

## Warnings

- Don't conflate FPT derandomization with kernel derandomization; deterministic OCT FPT algorithms are old news.
- "Polynomial kernel" must be polynomial in *k*, not n; also verify output is again an OCT instance (a "compression" to a different problem with a known polynomial back-translation also counts as a kernel by standard composition arguments).
- Schwartz–Zippel cannot be removed by naive repetition or pairwise independence; the random objects are field elements in an exponentially large field, and the failure event is structural (wrong matroid), so standard small-sample-space techniques don't obviously apply.
- Check recent literature before claiming novelty: partial derandomizations for related problems (e.g., Edge Bipartization above/below guarantees, or OCT with structural promises) may have appeared after 2015 — status "(verify)" as of this brief's knowledge.

---
*`ALGO-000096` · unverified*

## Acceptance guidance

Any of the following would resolve the problem:

1. **Construction**: a deterministic polynomial-time algorithm mapping any OCT instance (G, k) to an equivalent instance of size k^O(1).
2. **Reduction-rule approach**: a set of provably safe reduction rules yielding the same bound.
3. **Partial progress that counts**: a deterministic kernel for OCT on any significantly broader graph class than H-minor-free; a deterministic kernel of size k^O(log k); or a derandomization of a well-defined subclass of the Kratsch–Wahlström matroid machinery.
4. **Negative direction**: a formal barrier, e.g., showing deterministic polynomial kernelization of OCT would imply deterministic constructions of certain matroid representations believed hard, or a kernel lower bound under a stronger assumption.

A referee would accept: full proof of correctness, polynomial running time, and explicit size bound, in a self-contained writeup.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/8303d580-61a6-4f38-bae0-0b5f986c4c4c?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000096.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000096)
