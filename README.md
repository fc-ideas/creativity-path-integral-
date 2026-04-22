# A Path-Integral View of Creativity via Abstract Graph Decompositions

## Abstract

We propose a formal, operational notion of creativity grounded in future generativity rather than immediate utility. An idea is deemed creative to the extent that it enables the efficient generation of many future ideas that themselves have non-trivial value for downstream tasks. We formalize this intuition using a path-integral–like construction over possible future transformations, but acknowledge that exact computation is infeasible. We therefore introduce an approximate, task-anchored instantiation based on *abstract graph decompositions*, substitutional recomposition, and empirical value estimation via instance-level contribution measures such as Data Shapley or bias–variance–based diagnostics. The result is a practical framework for ranking ideas, representations, or decompositions by their *generative potential* rather than by static performance alone.



## 1. Motivation: Creativity as Future Reachability

Most evaluation frameworks in machine learning, science, and innovation are retrospective: an object (model, idea, representation) is evaluated by its immediate performance on a fixed task. Creativity, however, is inherently prospective. Historically, ideas we label as creative—calculus, germ theory, backpropagation, convolutional structure—are valuable not primarily because of their first application, but because they unlock large regions of future conceptual or practical space.

This motivates a shift from *instantaneous utility* to *future reachability*. Informally:

> An idea is creative if it serves as a stepping stone that enables many valuable future ideas.

This framing naturally suggests a dynamical and combinatorial perspective: ideas generate variants, variants generate further variants, and value accumulates along paths through this space.



## 2. Creativity as a Path Integral over Futures

Let $i_0$ be an initial idea. Consider all possible sequences of transformations, extensions, or recombinations that can be applied to it, yielding a tree (or graph) of future ideas $\{ i_t \}$. Let $V(i)$ denote a task-dependent value function that assigns utility to an idea or instance.

The *ideal* creativity score of $i_0$ can be written abstractly as a path integral:

$$\mathcal{C}(i_0)=\int_{\text{all future paths } \pi}\left(\sum_{t \ge 0} \gamma^t \, V(i_t)\right)\, d\mu(\pi),$$

where:

* paths $\pi = (i_0, i_1, i_2, \dots)$ represent possible future developments,
* $\mu$ is a measure over transformation sequences,
* $\gamma \in (0,1]$ discounts distant futures.

This definition captures three essential aspects:

1. **Non-locality in time**: creativity depends on long-term consequences.
2. **Unequal futures**: not all descendants are equally valuable.
3. **Generativity**: branching structure matters, not just best-case outcomes.



## 3. The Intractability of Exact Evaluation

Direct evaluation of $\mathcal{C}(i_0)$ is infeasible:

* The future space is combinatorially explosive.
* The value function depends on unknown future tasks.
* The true dynamics of idea generation require a “world simulator.”

However, exact evaluation is unnecessary if the goal is *ranking*. If an approximate model preserves the ordering of creativity scores with high probability, it is sufficient for comparative evaluation and selection.

This motivates a restricted, domain-specific approximation that trades completeness for tractability.



## 4. Abstract Graphs as a Generative Substrate

We instantiate ideas as graphs and introduce **abstract graphs** as a mechanism for controlled decomposition and recomposition.

An abstract graph consists of:

* A *pre-image graph* $G$, representing a concrete instance.
* A *decomposition* $D$ that partitions $G$ into subgraphs (parts).
* An *image graph* $\mathcal{A}_D(G)$, whose nodes correspond to these parts and whose edges encode their relations.

Crucially, a decomposition $D$ defines:

* What counts as a reusable “conceptual part.”
* Which substitutions are admissible.
* The space of instances reachable via recomposition.

Thus, the decomposition itself becomes the object whose creativity we wish to evaluate.



## 5. Creativity of a Decomposition

Given a decomposition $D$, we define a generative process:

1. Decompose a corpus of graphs using $D$.
2. Form a library of parts.
3. Generate new graphs by substituting, recombining, or perturbing parts while respecting the abstract structure.

As a concrete instantiation, we use a simple genetic algorithm over decomposed parts. At each step, we sample two generated instances, sampling them with probability proportional to $V$, and exchange compatible parts under $D$ to produce offspring. This keeps recomposition anchored to the abstract structure while biasing search toward higher-value regions.

This induces a *local future space*:
$$\mathcal{N}_D(G) = \{ G' : G' \text{ reachable from } G \text{ by recomposition under } D  \text{ in a finite number of steps} \}.$$

The creativity of $D$ is then linked to:

* The size and diversity of $\mathcal{N}_D$.
* The fraction of generated instances that are valuable.
* The efficiency with which high-value instances are reached.



## 6. Approximating the Path Integral

Within this restricted space, the path integral becomes computable:

$$\mathcal{C}(D) \approx \mathbb{E}_{G' \sim \mathcal{G}_D} \big[ V(G') \big],$$

where $\mathcal{G}_D$ is the distribution over generated graphs induced by $D$.

Rather than enumerating all paths, we sample recompositions and estimate:

* Expected value,
* Value variance,
* Tail mass of highly valuable instances.

This yields a practical proxy for generative potential.



## 7. Valuing Generated Instances

To ground $V(G')$, we tie creativity to *task performance*. Two complementary approaches are particularly natural:

### 7.1 Data Shapley–Style Contributions

Treat generated instances as candidate data points for a downstream learner. The value of an instance is its marginal contribution to task performance:

$$V(G') \approx \text{ShapleyValue}(G'; \mathcal{T}),$$

where $\mathcal{T}$ denotes the task or model class. This directly measures how useful a generated instance is in improving generalization.

### 7.2 Bias–Variance Diagnostics

Alternatively, assess how instances affect bias, variance, or calibration of a learner. Instances that reduce variance without increasing bias (or that expand decision boundaries) are deemed more valuable.

Both views emphasize *usefulness as an enabler*, not mere novelty.



## 8. Ranking Decompositions by Creative Quality

Putting everything together, a decomposition $D$ is creative if:

* It induces a rich recomposition space.
* A non-trivial fraction of generated instances are high-value.
* Valuable instances are reachable in few recomposition steps.
* Generated instances generalize across tasks, not just a single benchmark.

This yields an empirical ranking:
$$D_1 \succ D_2 \quad \text{if} \quad \mathcal{C}(D_1) > \mathcal{C}(D_2).$$

Importantly, this ranking compares *ways of carving the world*, not just individual artifacts.



## 9. Interpretation and Implications

Under this framework:

* Creativity is a property of representations and decompositions, not isolated outputs.
* Good abstractions are those that make useful futures dense and accessible.
* Evaluation shifts from “does this work?” to “what does this make possible?”

This perspective naturally explains why certain paradigms dominate: they compress structure in a way that maximizes downstream recombinability and value density.



## 10. Outlook

The proposed framework opens several directions:

* Learning decompositions that maximize generative potential.
* Comparing symbolic, neural, and hybrid abstractions on equal footing.
* Studying creativity as a phase transition in reachable value mass.

Ultimately, this approach reframes creativity as a measurable, operational property tied to future reachability under constrained but meaningful generative dynamics.
