---
layout: post
title: "Deep Learning Reading List, Part 2: Attention, Transformers and What They Enabled"
date: 2026-10-27
section: Tech
description: From Bahdanau attention to the Transformer, then the models that reuse attention for new problems, then the papers about scale.
tags: [deep-learning, reading-list, paper-review, attention, transformers]
---
This is **Part 2 of 3** in my reading list of foundational deep learning papers. I'm a data scientist who is new to AI, and I'm writing for others in the same position: people who know regression, regularization, feature engineering and cross-validation, and want to see how the deep learning papers connect to them.

This post covers attention, the Transformer, and what came after. If you haven't read it yet, [Part 1]({% post_url 2026-10-26-deep-learning-reading-list-foundations %}) covers the CNN and RNN foundations that this post builds on.

## Why this order

This block comes after Part 1 because attention is introduced as an upgrade to RNN encoder-decoders, and the later papers lean on both CNN and RNN ideas. Within the post the order has three steps. Attention (Bahdanau) comes first, followed by the Transformer ("Attention Is All You Need") and its annotated implementation. Then come the models that reuse attention for new problems: pointer networks, sets, memory, relational reasoning and graphs. The post finishes with scale: GPipe, Deep Speech 2, VLAE and scaling laws.

Next in the series: [Part 3, Information theory and complexity]({% post_url 2026-10-28-deep-learning-reading-list-information-theory %}).

---

### Neural Machine Translation by Jointly Learning to Align and Translate {#bahdanau}

*Dzmitry Bahdanau, et al.* [Paper](https://arxiv.org/abs/1409.0473)

**Read first:** [Seq2Seq](https://arxiv.org/abs/1409.3215) (Sutskever et al., 2014); [GRU / RNN Encoder-Decoder](https://arxiv.org/abs/1406.1078) (Cho et al., 2014); [LSTM](https://www.bioinf.jku.at/publications/older/2604.pdf) (1997).

The authors remove the fixed-length bottleneck of encoder-decoder translation. Instead of squeezing a whole source sentence into one vector, the decoder learns a soft alignment and attends over all of the encoder's states at each output step. This was the first widely used attention mechanism, and it improves translation of long sentences. One way to think about it is as a learned, differentiable weighted average: at each step the model decides how much each input position matters, much like a kernel-weighted average in nonparametric regression, where the weights are learned.

*Why it's at this point in the order:* it is the starting point for everything else in this post, and it directly upgrades the RNN encoder-decoders of Part 1.

### Attention Is All You Need {#transformer}

*Ashish Vaswani, et al.* [Paper](https://arxiv.org/abs/1706.03762)

**Read first:** [Bahdanau attention](#bahdanau) ★; [Luong attention](https://arxiv.org/abs/1508.04025) (2015); [Layer Normalization](https://arxiv.org/abs/1607.06450) (Ba et al., 2016).

This paper introduces the Transformer, which drops recurrence and convolution and relies only on self-attention. It uses multi-head attention and positional encodings, which are needed because attention on its own has no notion of order. It set new translation records, trained much faster, and underlies most modern language models. Training is faster because, without recurrence, the positions in a sequence can be processed in parallel instead of one step at a time.

*Why it's at this point in the order:* it generalizes the attention idea from Bahdanau and makes it the whole architecture.

### The Annotated Transformer {#annotated-transformer}

*Sasha Rush, et al.* [Walkthrough](https://nlp.seas.harvard.edu/annotated-transformer/)

**Read first:** [Seq2Seq](https://arxiv.org/abs/1409.3215) (Sutskever et al., 2014); [Bahdanau attention](#bahdanau) ★; [Luong attention](https://arxiv.org/abs/1508.04025) (2015).

A line-by-line PyTorch walkthrough of "Attention Is All You Need". It interleaves the paper's text with working code for the encoder-decoder, multi-head attention, positional encodings and the training loop. If you learn best by reading code, this is where the paper becomes concrete. Consider reading it alongside the paper rather than after.

*Why it's at this point in the order:* it follows the paper it annotates, and it gives you a working reference before the later papers reuse the same pieces.

### Pointer Networks {#pointer-networks}

*Oriol Vinyals, et al.* [Paper](https://arxiv.org/abs/1506.03134)

**Read first:** [Seq2Seq](https://arxiv.org/abs/1409.3215) (2014); [Bahdanau attention](#bahdanau) ★; [LSTM](https://www.bioinf.jku.at/publications/older/2604.pdf) (1997).

Pointer Networks use attention as a pointer to select positions in the input, so the output "vocabulary" varies with the input length. Applied to convex hulls, Delaunay triangulation and the traveling salesman problem, they do well and generalize across input sizes. The key idea is that the output is not a word from a fixed list, but an index into the input.

*Why it's at this point in the order:* it is the first of the papers that reuse attention for a new kind of problem.

### Order Matters: Sequence to Sequence for Sets {#order-matters}

*Oriol Vinyals, et al.* [Paper](https://arxiv.org/abs/1511.06391)

**Read first:** [Seq2Seq](https://arxiv.org/abs/1409.3215) (2014); [Bahdanau attention](#bahdanau) ★; [Pointer Networks](#pointer-networks) ★.

The paper shows that input and output order strongly affects seq2seq models even when the data is naturally a set. It introduces an attention-based read-process-write architecture for unordered input, and a method for searching over output orderings. For anyone used to tabular data, where row order doesn't matter, this is a useful reminder that sequence models quietly assume order does matter.

*Why it's at this point in the order:* it extends the pointer idea to sets, so it needs Pointer Networks first.

### Neural Turing Machines {#ntm}

*Alex Graves, et al.* [Paper](https://arxiv.org/abs/1410.5401)

**Read first:** [LSTM](https://www.bioinf.jku.at/publications/older/2604.pdf) (1997); [Seq2Seq](https://arxiv.org/abs/1409.3215) (2014); [Bahdanau attention](#bahdanau) ★.

The paper couples a neural network controller to an external memory through differentiable attention-based read and write heads, trainable end to end. It learns simple algorithms such as copying, sorting and associative recall, and generalizes to longer sequences. The memory is separate from the controller, which is the point: the model can store and retrieve information without having to fit everything into its hidden state.

*Why it's at this point in the order:* it uses attention as a memory-addressing mechanism, which builds on the attention idea but pushes it in a different direction.

### A Simple Neural Network Module for Relational Reasoning {#relation-networks}

*Adam Santoro, et al.* [Paper](https://arxiv.org/abs/1706.01427)

**Read first:** [CLEVR](https://arxiv.org/abs/1612.06890) (Johnson et al., 2016); [Interaction Networks](https://arxiv.org/abs/1612.00222) (Battaglia et al., 2016); [CS231n notes]({% post_url 2026-10-26-deep-learning-reading-list-foundations %}#cs231n) ★ (Part 1).

Relation Networks apply a shared function to all pairs of objects and aggregate the results, which builds relational reasoning into the model. The module reaches superhuman accuracy on CLEVR visual question answering and solves bAbI text tasks. The design is a good example of putting structure into a model instead of hoping it will be learned, in the same spirit as choosing features that encode what you know about a problem.

*Why it's at this point in the order:* it needs the CNN background from Part 1 and the dataset and interaction-network context it lists, and it leads into the relational recurrent paper that follows.

### Relational Recurrent Neural Networks {#relational-rnn}

*Adam Santoro, et al.* [Paper](https://arxiv.org/abs/1806.01822)

**Read first:** [Relation Networks](#relation-networks) ★; [Attention Is All You Need](#transformer) ★; [LSTM](https://www.bioinf.jku.at/publications/older/2604.pdf) (1997).

The paper adds a Relational Memory Core, in which memory slots interact through multi-head self-attention. This helps on tasks that need reasoning over remembered information. It beats LSTMs on reinforcement learning, program evaluation and language modeling.

*Why it's at this point in the order:* it combines relational reasoning with the Transformer's self-attention inside a recurrent model, so it needs both papers first.

### Neural Message Passing for Quantum Chemistry {#mpnn}

*Justin Gilmer, et al.* [Paper](https://arxiv.org/abs/1704.01212)

**Read first:** [Neural Fingerprints](https://arxiv.org/abs/1509.09292) (Duvenaud et al., 2015); [Gated Graph Sequence NNs](https://arxiv.org/abs/1511.05493) (Li et al., 2015); [Graph Convolutional Networks](https://arxiv.org/abs/1609.02907) (Kipf & Welling, 2016).

The paper unifies several graph neural networks under a Message Passing Neural Network framework with message, update and readout functions. On QM9 molecular property prediction, the best variants reach chemical accuracy on most targets. Molecules are graphs, not tables or images, and this framework is a general way to learn from graph-structured data.

*Why it's at this point in the order:* it carries the idea of passing information between related items from the relational papers over to graphs.

### Variational Lossy Autoencoder {#vlae}

*Xi Chen, et al.* [Paper](https://arxiv.org/abs/1611.02731)

**Read first:** [Auto-Encoding Variational Bayes](https://arxiv.org/abs/1312.6114) (Kingma & Welling, 2013); [PixelRNN](https://arxiv.org/abs/1601.06759) (van den Oord et al., 2016); [Generating Sentences from a Continuous Space](https://arxiv.org/abs/1511.06349) (Bowman et al., 2015).

The authors address a problem where powerful autoregressive decoders cause VAEs to ignore their latent codes. They restrict the decoder's receptive field so the latent variable must capture global structure, and they use an autoregressive prior. This gives state-of-the-art density estimation on several image datasets. If you know PCA, a latent code plays a similar role: a compressed representation of the data that is supposed to capture its main structure. Here the decoder is so strong that it can ignore that representation unless the model is designed to prevent it.

*Why it's at this point in the order:* it sits with the scale-and-capacity papers, and it also points ahead to the compression ideas in Part 3.

### Deep Speech 2: End-to-End Speech Recognition in English and Mandarin {#deep-speech-2}

*Dario Amodei, et al.* [Paper](https://arxiv.org/abs/1512.02595)

**Read first:** [CTC](https://www.cs.toronto.edu/~graves/icml_2006.pdf) (Graves et al., 2006); [Deep Speech](https://arxiv.org/abs/1412.5567) (Hannun et al., 2014); [Batch Normalization](https://arxiv.org/abs/1502.03167) (2015).

An end-to-end deep learning system replaces hand-engineered speech pipelines with neural networks trained with CTC. The same architecture handles English and Mandarin. HPC techniques such as GPU batch dispatch make training much faster. This is feature engineering being replaced by learned features, at the scale of a whole production system.

*Why it's at this point in the order:* it shows what scaling an end-to-end system looks like, as a bridge to the scaling papers that follow.

### GPipe: Easy Scaling with Micro-Batch Pipeline Parallelism {#gpipe}

*Yanping Huang, et al.* [Paper](https://arxiv.org/abs/1811.06965)

**Read first:** [Large Scale Distributed Deep Networks](https://papers.nips.cc/paper/2012/hash/6aca97005c68f1206823815f66102863-Abstract.html) (Dean et al., 2012); [AlexNet]({% post_url 2026-10-26-deep-learning-reading-list-foundations %}#alexnet) ★ (Part 1); [ResNet]({% post_url 2026-10-26-deep-learning-reading-list-foundations %}#resnet) ★ (Part 1).

GPipe is a library for training very large networks by partitioning layers across accelerators. It pipelines micro-batches through the partitions and uses re-materialization to save memory. It achieves near-linear speedup and trains a 557M-parameter AmoebaNet to state-of-the-art ImageNet accuracy.

*Why it's at this point in the order:* it needs the Part 1 image models it scales up, and it introduces the engineering side of scale.

### Scaling Laws for Neural Language Models {#scaling-laws}

*Jared Kaplan, et al.* [Paper](https://arxiv.org/abs/2001.08361)

**Read first:** [Attention Is All You Need](#transformer) ★; [Deep Learning Scaling is Predictable, Empirically](https://arxiv.org/abs/1712.00409) (Hestness et al., 2017); [GPT-3](https://arxiv.org/abs/2005.14165) (Brown et al., 2020).

The authors find that language model loss follows smooth power laws in model size, dataset size and training compute across many orders of magnitude. Architecture details matter little by comparison. Larger models are more sample-efficient, and compute-optimal training favors very large models stopped well before convergence. This is closest in spirit to the learning curves you may have drawn when deciding whether to collect more data or use a bigger model.

*Why it's at this point in the order:* it is the capstone of the post, and it depends on the Transformer and the scale papers before it.

---

## Summary table

| Paper | Author | Link | Read first |
|---|---|---|---|
| [Bahdanau attention](#bahdanau) | Bahdanau, et al. | [arXiv](https://arxiv.org/abs/1409.0473) | Seq2Seq, GRU, LSTM |
| [Attention Is All You Need](#transformer) | Vaswani, et al. | [arXiv](https://arxiv.org/abs/1706.03762) | Bahdanau, Luong, LayerNorm |
| [The Annotated Transformer](#annotated-transformer) | Rush, et al. | [Site](https://nlp.seas.harvard.edu/annotated-transformer/) | Seq2Seq, Bahdanau, Luong |
| [Pointer Networks](#pointer-networks) | Vinyals, et al. | [arXiv](https://arxiv.org/abs/1506.03134) | Seq2Seq, Bahdanau, LSTM |
| [Order Matters](#order-matters) | Vinyals, et al. | [arXiv](https://arxiv.org/abs/1511.06391) | Seq2Seq, Bahdanau, Pointer Nets |
| [Neural Turing Machines](#ntm) | Graves, et al. | [arXiv](https://arxiv.org/abs/1410.5401) | LSTM, Seq2Seq, Bahdanau |
| [Relation Networks](#relation-networks) | Santoro, et al. | [arXiv](https://arxiv.org/abs/1706.01427) | CLEVR, Interaction Nets, CS231n |
| [Relational RNNs](#relational-rnn) | Santoro, et al. | [arXiv](https://arxiv.org/abs/1806.01822) | Relation Nets, Transformer, LSTM |
| [Message Passing (MPNN)](#mpnn) | Gilmer, et al. | [arXiv](https://arxiv.org/abs/1704.01212) | Neural Fingerprints, GG-NN, GCN |
| [Variational Lossy Autoencoder](#vlae) | Chen, et al. | [arXiv](https://arxiv.org/abs/1611.02731) | VAE, PixelRNN, Bowman et al. |
| [Deep Speech 2](#deep-speech-2) | Amodei, et al. | [arXiv](https://arxiv.org/abs/1512.02595) | CTC, Deep Speech, BatchNorm |
| [GPipe](#gpipe) | Huang, et al. | [arXiv](https://arxiv.org/abs/1811.06965) | DistBelief, AlexNet, ResNet |
| [Scaling Laws](#scaling-laws) | Kaplan, et al. | [arXiv](https://arxiv.org/abs/2001.08361) | Transformer, Hestness, GPT-3 |

## Next up

[Part 3: Information theory and complexity]({% post_url 2026-10-28-deep-learning-reading-list-information-theory %}) steps back from architectures to ask why compression, simplicity and generalization are connected. It opens with Hinton and van Camp, a neural network paper that is really an argument about description length.
