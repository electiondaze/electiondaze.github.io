---
layout: post
title: "Deep Learning Reading List, Part 3: Information Theory and Complexity"
date: 2026-10-28
section: Tech
description: Why compression, simplicity and generalization are connected, from the MDL principle to Kolmogorov complexity and a formal definition of intelligence.
tags: [deep-learning, reading-list, paper-review, information-theory, mdl]
---
This is **Part 3 of 3** in my reading list of foundational deep learning papers. I'm a data scientist who is new to AI, and this series is for readers with a similar background: comfortable with regression, regularization, PCA and cross-validation, and curious about how the deep learning literature fits together.

This post covers the theoretical end of the path: information theory and complexity. Earlier parts covered [Foundations]({% post_url 2026-10-26-deep-learning-reading-list-foundations %}) and [Attention and Transformers]({% post_url 2026-10-27-deep-learning-reading-list-attention-transformers %}).

## Why this order

These papers (Aaronson, MDL, Kolmogorov, Legg) are mostly independent of the rest of the list and can be read in parallel with it. They go last because they are more theoretical, and they make more sense once you have seen real models overfit and generalize.

Hinton and van Camp opens this post, and the choice deserves a note. In my original table this paper sat right after RNN Regularization, because both are about keeping networks from overfitting. Here it is instead the bridge between the neural network papers in Parts 1 and 2 and information theory. It is a neural network paper, but its argument is really about minimum description length, so it is the natural way in.

The common thread for a data scientist is a familiar idea in new language: a model that explains the data with a shorter description is usually the one that generalizes. That is the same instinct as regularization, Occam's razor and penalized model selection.

---

### Keeping Neural Networks Simple by Minimizing the Description Length of the Weights {#hinton-van-camp}

*Geoffrey E. Hinton and Drew van Camp.* [Paper](https://www.cs.toronto.edu/~hinton/absps/colt93.pdf)

**Read first:** [A Mathematical Theory of Communication](https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf) (Shannon, 1948); [MDL Tutorial](#mdl-tutorial) ★ (same post); [Backprop](https://www.nature.com/articles/323533a0) (1986).

The authors argue that a network generalizes better when its weights can be described with few bits. They penalize the information content of the weights by adding Gaussian noise to them and minimizing the description length. This is an early variational, Bayesian view of regularization. If you know ridge regression or lasso, the spirit is the same: a penalty that discourages the model from using more precision than the data justifies. Here the penalty comes from an information-theoretic argument instead of being added by hand.

*Why it's at this point in the order:* it is the bridge from the neural network papers of Parts 1 and 2 to the information-theoretic ideas in the rest of this post.

### A Tutorial Introduction to the Minimum Description Length Principle {#mdl-tutorial}

*Peter Grünwald.* [Tutorial](https://arxiv.org/abs/math/0406077)

**Read first:** [Shannon, 1948](https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf); [Kolmogorov Complexity book](#kolmogorov-book) ★ (intro chapters).

A tutorial on the MDL principle: the best model is the one that compresses the data most. It covers refined MDL, universal codes, two-part and normalized maximum likelihood codes, and the links to Bayesian inference and model selection. For a data scientist the practical connection is model selection. MDL gives a principled way to trade off fit against complexity, in the same family of ideas as AIC, BIC and cross-validation.

*Why it's at this point in the order:* it states the principle that Hinton and van Camp apply, and it prepares the ground for Kolmogorov complexity.

### Kolmogorov Complexity and Algorithmic Randomness {#kolmogorov-book}

*A. Shen, V. A. Uspensky, N. Vereshchagin.* [Book](https://www.lirmm.fr/~ashen/kolmbook-eng-scan.pdf)

**Read first:** [Shannon, 1948](https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf); basic probability and computability (for example, Turing machines).

A textbook on algorithmic information theory. It covers plain and prefix complexity, the incompressibility method, algorithmic randomness for finite and infinite sequences, and applications to computability and combinatorics. The core idea is to measure the complexity of an object by the length of the shortest program that produces it. This is a textbook, so you do not need to read it cover to cover. The introductory chapters are what the other papers in this post assume.

*Why it's at this point in the order:* it is the foundation for Aaronson's complexity papers and for Legg's thesis, which both rely on its intro chapters.

### The First Law of Complexodynamics {#complexodynamics}

*Scott Aaronson.* [Blog post](https://scottaaronson.blog/?p=762)

**Read first:** [Shannon, 1948](https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf); [Kolmogorov Complexity book](#kolmogorov-book) ★ (intro chapters).

This blog post asks why the "complexity" of a closed physical system rises and then falls, while entropy only increases. Aaronson proposes a resource-bounded form of Kolmogorov complexity ("complextropy") to capture the interesting middle stage of cream mixing into coffee. The question is intuitive even without the formal machinery: a cup of coffee with cream just poured in is more interesting to describe than either the unmixed or the fully mixed state.

*Why it's at this point in the order:* it applies the Kolmogorov idea to a concrete question, and it motivates the follow-up paper.

### Quantifying the Rise and Fall of Complexity in Closed Systems: The Coffee Automaton {#coffee-automaton}

*Scott Aaronson, et al.* [Paper](https://arxiv.org/abs/1405.6903)

**Read first:** [The First Law of Complexodynamics](#complexodynamics) ★; [Kolmogorov Complexity book](#kolmogorov-book) ★; [Shannon, 1948](https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf).

The paper follows up on the complexodynamics idea with a cellular-automaton model of mixing coffee and cream. The authors define "apparent complexity" using compressed, coarse-grained descriptions and show empirically that it rises and then falls as the system evolves. In practice, a compressor stands in for the uncomputable ideal, which is a pattern you may recognize from using compressed size as a rough measure of complexity.

*Why it's at this point in the order:* it is the empirical follow-up to the blog post, so it has to come after it.

### Machine Super Intelligence {#machine-super-intelligence}

*Shane Legg.* [Thesis](https://www.vetta.org/documents/Machine_Super_Intelligence.pdf)

**Read first:** [Universal Intelligence](https://arxiv.org/abs/0712.3329) (Legg & Hutter, 2007); [MDL Tutorial](#mdl-tutorial) ★; [Kolmogorov Complexity book](#kolmogorov-book) ★.

Legg's PhD thesis studies how to define and measure intelligence. It uses Kolmogorov complexity and Solomonoff induction to give a formal definition of universal intelligence. It then examines theoretical limits on prediction and the prospects for, and risks of, superintelligent machines.

*Why it's at this point in the order:* it is the most ambitious paper on the list, and it builds on both the MDL and Kolmogorov material, so it goes last.

---

## Summary table

| Paper | Author | Link | Read first |
|---|---|---|---|
| [Keeping Neural Networks Simple](#hinton-van-camp) | Hinton, van Camp | [PDF](https://www.cs.toronto.edu/~hinton/absps/colt93.pdf) | Shannon, MDL Tutorial, Backprop |
| [MDL Tutorial](#mdl-tutorial) | Grünwald | [arXiv](https://arxiv.org/abs/math/0406077) | Shannon, Kolmogorov book |
| [Kolmogorov Complexity and Algorithmic Randomness](#kolmogorov-book) | Shen, Uspensky, Vereshchagin | [PDF](https://www.lirmm.fr/~ashen/kolmbook-eng-scan.pdf) | Shannon, probability, computability |
| [First Law of Complexodynamics](#complexodynamics) | Aaronson | [Post](https://scottaaronson.blog/?p=762) | Shannon, Kolmogorov book |
| [The Coffee Automaton](#coffee-automaton) | Aaronson, et al. | [arXiv](https://arxiv.org/abs/1405.6903) | Complexodynamics, Kolmogorov book, Shannon |
| [Machine Super Intelligence](#machine-super-intelligence) | Legg | [PDF](https://www.vetta.org/documents/Machine_Super_Intelligence.pdf) | Universal Intelligence, MDL, Kolmogorov book |

## Next up

This is the last post in the series. If you're reading in order, a natural next step is to go back to [Part 1]({% post_url 2026-10-26-deep-learning-reading-list-foundations %}) or [Part 2]({% post_url 2026-10-27-deep-learning-reading-list-attention-transformers %}) with the description-length lens in mind: ask of each model how much information it needs to describe what it has learned.
