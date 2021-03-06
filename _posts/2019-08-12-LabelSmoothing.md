---
layout: post
title: Label Smoothing
category: Machine Learning
tags:
- machinelearning
---

Working in machine learning includes dealing with poorly labelled datasets. Very few companies can afford hiring people to label the huge amounts of data required for large scale projects. Luckily, high quality datasets are available for practise projects. In production however, one will most likely need a custom dataset. With applications such as [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) and [Scrapy](https://scrapy.org) it's easy to collect large amounts of data from the internet. Labelling the data is a common pain-point. Luckily there are other solutions available than annotating the data yourself or hiring people to do it for you. Label smoothing is a mathematical technique that helps machine learning models to deal with data where some labels are wrong.

## The problem with the approach

### Cross entropy loss for binary classification

A commonly used loss function for logistic regression is cross-entropy loss. For binary classification problems ($$m = 2$$) it is defined as follows:

$$- \frac{1}{n}\displaystyle\sum_{i=1}^{n}-y^{(i)}\log h(x^{(i)})-(1-y^{(i)})\cdot\log(1-h_\theta(x^{(i)}))$$

 where $$n$$ is the size of the training set.

While this might seem like a complicated mathematical function, it is really not. By breaking it up into parts, it's quite easy to understand. Note every $$y^{(i)} \in \{0, 1\}$$.

$$-y^{(i)}\log h(x^{(i)}) -(1-y^{(i)})$$ only affects labels where $$y = 1$$. It takes the prediction $$\log h(x^{(i)})$$ and compares it to $$1 - y^{(i)}$$. If the model predicted the correct class 1, the loss would be $$-1 \cdot \log (1) - (1-1) = 0$$. As the model gets more certain about an example belonging to the incorrect class, $$\log x$$ and consequently the loss will increase.

The second part of the equation is really the same thing as the previous part except for the negative examples. Negative examples have $$y^{(i)} = 0$$, hence $$(1 - y^{(i)})$$.

You should note that because $$y^{(i)} \in \{0, 1\}$$ one of the parts will be 0 so it is not taken into account when computing the gradients of the loss

### Mislabelled examples

Assuming our model is able to classify most items correctly, in the case a training example is mislabelled, the model will predict the "wrong" label resulting in a high penalty.This is obviously something we want to avoid.

## How label smoothing can help

Label smoothing is a variation on one hot encoding where the negative labels have a value slightly higher than $$0$$, say $$0.1$$ and the positive labels have a value slightly lower than $$1$$, say $$0.9$$.

When I first learned about this, it seemed like a weird idea to me that was not going to work. After reading more however, it seemed like a brilliant idea.

One should see the values $$0$$ and $$1$$ as simply true and false. Taking values that vary slightly from the classic values, they are a more nuanced way to describing the data. $$0.1$$ could be viewed as: "there is a very low chance this data is \<one of two classes\>" whereas $$0.9$$ is, of course, a high chance.

Using these "nuanced" labels, the cost of an incorrect prediction is slightly lower than using "hard" labels resulting in a smaller gradient. While this intuition helped me understand why it could be a good idea, I was not entirely convinced it would work in an application because the loss is lowered for all wrong classifications. So I decided to read some about some research done on the subject. A table copied from "When Does Label Smoothing Help?" (see "Further Reading" below):

![results of using label smoothing on some models](/assets/images/smoothing.png)

Mathematically, one can work out the aforementioned equation to see the loss decrease when using smoothed labels. The part of the equation that would previously be set to 0, now has a non-zero value.

## In Python

To use label smoothing, one needs to convert your one hot encoded array of floating point numbers to the "nuanced" version. In Numpy, one can do so using `np.where` indexing.

```python
y[np.where(y == 0)] = 0.1
y[np.where(y == 1)] = 0.9
```

The labels are now smoothed and the neural network will handle everything else automatically. That is the beauty of using label smoothing; it is incredibly easy.

## For multiclass classifcation

Label smoothing can be used for multi class classification as well as long as the labels are one hot encoded.

A generic function for one hot encoding and smoothing labels:

```python
epsilon = 0.1

def remap(y, K):
  """ One hot encode labels `y` over `K` classes. `y` should be of the form [1, 6, 3, etc.] """
  m = len(y)
  out = np.ones((m, K)) * epsilon
  for index in range(m):
      out[index][y[index] - 1] = 1 - epsilon
  return out
```

## Further reading

A recent study by Rafael Müller, Simon Kornblith and Geoffrey Hintin called "When Does Label Smoothing Help?" published only 2 months ago shows some positive results of label smoothing and discusses whether or not label smoothing should be used. Read the paper on [arXiv](https://arxiv.org/abs/1906.02629).

[This](https://www.reddit.com/r/MachineLearning/comments/73dfrh/p_noisy_labels_and_label_smoothing/) Reddit post going alongside [this](https://github.com/Kyubyong/label_smoothing) GitHub project has some very experiments on label smoothing.

<p class="text-muted">A huge thanks to <a target="_blank" href="https://twitter.com/miserendino_sam">Sam Miserendino</a> for proofreading this post!</p>
