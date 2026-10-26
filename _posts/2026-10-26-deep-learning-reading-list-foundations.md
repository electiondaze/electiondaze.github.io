---
layout: post
title: "Deep Learning Reading List, Part 1: Foundations"
date: 2026-10-26
section: Tech
description: Vision, sequences and the training tricks that make deep nets work, in an order where every paper follows the ones it depends on.
tags: [deep-learning, reading-list, paper-review]
---
I'm a data scientist who is new to the AI field, and I'm working through a reading list of foundational deep learning papers. This series is my notebook. If you come from regression, feature engineering and cross-validation, as I do, it is written for you: I'll try to tie each paper back to things you already know.

This is **Part 1 of 3**, and it covers the foundations: convolutional networks for images, recurrent networks for sequences, and the regularization and architecture tricks that made deep networks trainable. Part 2 covers attention and Transformers, and Part 3 covers information theory and complexity.

## Why this order

The whole list is arranged so that every paper comes after the papers it depends on. This block comes first because it supplies the vocabulary that every later paper assumes: backpropagation, CNNs, recurrence and gates. I start with the friendliest on-ramps (the CS231n notes, Karpathy's RNN post and Olah's LSTM post). Then come the papers that built on them: AlexNet, RNN regularization, ResNet and its follow-up, and dilated convolutions.

Next in the series: [Part 2, Attention, Transformers and what they enabled]({% post_url 2026-10-27-deep-learning-reading-list-attention-transformers %}).

---

### CS231n: Convolutional Neural Networks for Visual Recognition {#cs231n}

*Fei-Fei Li, Andrej Karpathy, Justin Johnson, et al. (Stanford).* [Course notes](https://cs231n.github.io/)

**Read first:** [Learning representations by back-propagating errors](https://www.nature.com/articles/323533a0) (Rumelhart et al., 1986); [Gradient-Based Learning Applied to Document Recognition](http://yann.lecun.com/exdb/publis/pdf/lecun-98.pdf) (LeCun et al., 1998).

These are the notes for Stanford's deep learning for vision course. They cover image classification, loss functions, backpropagation, CNN architectures, training tips and transfer learning. For a data scientist the early material will feel familiar: a linear classifier with a loss function is logistic regression or an SVM in different clothes. What changes is that the features are learned instead of hand-engineered, and that is the through-line for the rest of the list. The notes are hands-on, so they work well as a first introduction to the field.

*Why it's at this point in the order:* it is the friendliest on-ramp to the field, and it supplies the vocabulary that the later papers assume.

### The Unreasonable Effectiveness of Recurrent Neural Networks {#rnn-effectiveness}

*Andrej Karpathy.* [Blog post](https://karpathy.github.io/2015/05/21/rnn-effectiveness/)

**Read first:** [Backprop](https://www.nature.com/articles/323533a0) (Rumelhart, 1986); [Finding Structure in Time](https://onlinelibrary.wiley.com/doi/abs/10.1207/s15516709cog1402_1) (Elman, 1990); [LSTM](https://www.bioinf.jku.at/publications/older/2604.pdf) (Hochreiter & Schmidhuber, 1997).

Karpathy trains character-level RNNs on Shakespeare, Wikipedia markup, LaTeX and Linux source code. The models produce plausible text and syntax, including matched brackets and indentation, and he visualizes what individual neurons learn. If you have worked with time series, think of an RNN as a model that carries a hidden state forward from step to step. Here that state is learned rather than specified by hand, as it would be in an ARIMA or a state-space model. The post is more about building intuition than about rigor, and that is its value.

*Why it's at this point in the order:* it is the other friendly on-ramp, and it introduces recurrence before the harder sequence papers.

### Understanding LSTM Networks {#understanding-lstm}

*Christopher Olah.* [Blog post](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)

**Read first:** [Finding Structure in Time](https://onlinelibrary.wiley.com/doi/abs/10.1207/s15516709cog1402_1) (Elman, 1990); [Backprop](https://www.nature.com/articles/323533a0) (Rumelhart, 1986).

An illustrated explanation of why vanilla RNNs struggle with long-term dependencies and how LSTMs address this. It walks through the cell state and the forget, input and output gates, and then surveys variants such as peepholes and GRUs. The way to read the gates is as learned, soft switches that decide what to keep, what to write and what to expose. The diagrams do most of the explaining.

*Why it's at this point in the order:* after Karpathy shows what RNNs can do, Olah explains the gating machinery that makes them work in practice.

### ImageNet Classification with Deep Convolutional Neural Networks (AlexNet) {#alexnet}

*Alex Krizhevsky, et al.* [Paper](https://papers.nips.cc/paper/2012/hash/c399862d3b9d6b76c8436e924a68c45b-Abstract.html)

**Read first:** [LeNet](http://yann.lecun.com/exdb/publis/pdf/lecun-98.pdf) (LeCun et al., 1998); [Backprop](https://www.nature.com/articles/323533a0) (1986); [CS231n notes](#cs231n) ★.

This is the AlexNet paper. A deep CNN trained on GPUs with ReLUs, dropout and data augmentation won ILSVRC-2012 by a large margin, and the result sparked the deep learning boom in computer vision. Two of its ingredients will sound familiar: dropout is a form of regularization, and data augmentation is a way of getting more training data without collecting any. The paper is a good example of an engineering recipe, with several simple pieces that work together.

*Why it's at this point in the order:* it needs the CNN background from CS231n and LeNet, and it is the paper that made that background matter.

### Recurrent Neural Network Regularization {#rnn-regularization}

*Wojciech Zaremba, et al.* [Paper](https://arxiv.org/abs/1409.2329)

**Read first:** [Dropout](https://jmlr.org/papers/v15/srivastava14a.html) (Srivastava et al., 2014); [LSTM](https://www.bioinf.jku.at/publications/older/2604.pdf) (1997).

The paper shows how to apply dropout correctly to LSTMs: only on the non-recurrent connections. This keeps the memory intact while reducing overfitting, and it improves language modeling, speech recognition, translation and image captioning. It is a good illustration of a general lesson. A regularizer that works well in one place can do damage in another, and you have to think about what the layer is for.

*Why it's at this point in the order:* it combines the LSTM from Olah's post with dropout, the regularization trick popularized by AlexNet.

### Deep Residual Learning for Image Recognition (ResNet) {#resnet}

*Kaiming He, et al.* [Paper](https://arxiv.org/abs/1512.03385)

**Read first:** [VGG](https://arxiv.org/abs/1409.1556) (Simonyan & Zisserman, 2014); [Batch Normalization](https://arxiv.org/abs/1502.03167) (Ioffe & Szegedy, 2015); [Highway Networks](https://arxiv.org/abs/1505.00387) (Srivastava et al., 2015).

The paper introduces residual connections, where layers learn a residual function that is added to an identity shortcut. This lets networks of over 100 layers train without degradation. ResNets won ILSVRC 2015 and became a foundational architecture. A loose analogy for a data scientist is boosting, where each stage only has to model what is left over from the previous stage. It is only an analogy and shouldn't be pushed too far.

*Why it's at this point in the order:* it builds on the deeper CNNs that AlexNet and VGG started, and it solves the problem of training them as they get deeper.

### Identity Mappings in Deep Residual Networks {#identity-mappings}

*Kaiming He, et al.* [Paper](https://arxiv.org/abs/1603.05027)

**Read first:** [ResNet](#resnet) ★; [Batch Normalization](https://arxiv.org/abs/1502.03167) (2015).

The paper analyzes signal propagation through residual blocks and shows that clean identity shortcuts work best. It proposes a "pre-activation" design with batch norm and ReLU placed before each convolution, which eases optimization and enables a 1001-layer network on CIFAR.

*Why it's at this point in the order:* it is a direct follow-up that only makes sense once you have read the original ResNet.

### Multi-Scale Context Aggregation by Dilated Convolutions {#dilated-convolutions}

*Fisher Yu and Vladlen Koltun.* [Paper](https://arxiv.org/abs/1511.07122)

**Read first:** [VGG](https://arxiv.org/abs/1409.1556) (2014); [Fully Convolutional Networks](https://arxiv.org/abs/1411.4038) (Long et al., 2015); [CS231n notes](#cs231n) ★.

The authors use dilated convolutions to grow the receptive field exponentially without losing resolution or adding parameters. A context module of stacked dilated layers improves dense prediction, including semantic segmentation on Pascal VOC. "Dense prediction" means producing an output for every pixel rather than one label per image, which is why keeping resolution matters.

*Why it's at this point in the order:* it needs the fully convolutional and CNN background from earlier, and it closes the vision half of this post with a different way of building on convolutions.

---

## Summary table

| Paper | Author | Link | Read first |
|---|---|---|---|
| [CS231n](#cs231n) | Li, Karpathy, Johnson, et al. | [Notes](https://cs231n.github.io/) | Backprop, LeNet |
| [Unreasonable Effectiveness of RNNs](#rnn-effectiveness) | Karpathy | [Post](https://karpathy.github.io/2015/05/21/rnn-effectiveness/) | Backprop, Elman, LSTM |
| [Understanding LSTM Networks](#understanding-lstm) | Olah | [Post](https://colah.github.io/posts/2015-08-Understanding-LSTMs/) | Elman, Backprop |
| [AlexNet](#alexnet) | Krizhevsky, et al. | [Paper](https://papers.nips.cc/paper/2012/hash/c399862d3b9d6b76c8436e924a68c45b-Abstract.html) | LeNet, Backprop, CS231n |
| [RNN Regularization](#rnn-regularization) | Zaremba, et al. | [arXiv](https://arxiv.org/abs/1409.2329) | Dropout, LSTM |
| [ResNet](#resnet) | He, et al. | [arXiv](https://arxiv.org/abs/1512.03385) | VGG, BatchNorm, Highway Nets |
| [Identity Mappings](#identity-mappings) | He, et al. | [arXiv](https://arxiv.org/abs/1603.05027) | ResNet, BatchNorm |
| [Dilated Convolutions](#dilated-convolutions) | Yu, Koltun | [arXiv](https://arxiv.org/abs/1511.07122) | VGG, FCN, CS231n |

## Next up

[Part 2: Attention, Transformers and what they enabled]({% post_url 2026-10-27-deep-learning-reading-list-attention-transformers %}) starts with Bahdanau's attention paper, which you can read as an upgrade to the RNN encoder-decoders from this post.
