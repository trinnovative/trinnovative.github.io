---
layout: post
title: Open Source Deep Learning Libraries
author: jschenkl
category: Data Science
tags: [Deep Learning, Data Science, Machine Learning, Neural Networks, Open Source]
comments: true
---

Deep Learning has recently gained a lot of attention. In January, Google announced that its deep learning powered system [AlphaGo has beaten the European Go Champion](http://googleresearch.blogspot.de/2016/01/alphago-mastering-ancient-game-of-go.html). The corresponding paper was published in [Nature](http://www.nature.com/nature/journal/v529/n7587/full/nature16961.html).
Besides that public media attendance, there's another recent trend in deep learning: There are more and more open source frameworks publicly available that can be used for building intelligent systems.

-----


### Framework Overview

The following (alphabetical) list provides an overview over some of the most common used frameworks available for deep learning. Most of them support calculations on CPU via CUDA or similar. Several of them are not limited to a specific programming language, but provided APIs or wrappers for several languages. 


#### [Caffe](http://caffe.berkeleyvision.org/)

Caffe describes itself as a "deep learning framework made with expression, speed, and modularity in mind". It is community driven and is developed at the Berkeley Vision and Learning Center. 

- Languages: Python, MATLAB, written in C++
- License: [BSD 2-Clause license](https://github.com/BVLC/caffe/blob/master/LICENSE)
- Github: [https://github.com/BVLC/caffe](https://github.com/BVLC/caffe)

#### [DL4J](http://deeplearning4j.org/)

DL4J is a library for deep learning written in Java and Scala targeting on enterprise usage. It supports several network types like Restricted Boltzmann machines, Convolutional Nets or Deep-belief networks out of the box. Furthermore, it integrates with [Hadoop](https://en.wikipedia.org/wiki/Apache_Hadoop) and [Spark](https://en.wikipedia.org/wiki/Apache_Spark).

- Languages: Java, Scala, Clojure
- License: [Apache 2.0](https://github.com/deeplearning4j/deeplearning4j/blob/master/LICENSE.txt)
- Github: [https://github.com/deeplearning4j/deeplearning4j](https://github.com/deeplearning4j/deeplearning4j)

#### [H2O](http://www.h2o.ai/)
H20 is a Java machine learning platform focused on big data and real-time analysis. Therefore, it also offers an integration with [Apache Spark](https://en.wikipedia.org/wiki/Apache_Spark)/[Hadoop](https://en.wikipedia.org/wiki/Apache_Hadoop) for distributed computation. Besides several programming APIs, there's also a web-based user interface available.

- Languages: R, Java, Scala, Python
- License: [Apache 2.0](https://github.com/h2oai/h2o-3/blob/master/LICENSE)
- Github: [https://github.com/h2oai/h2o-3](https://github.com/h2oai/h2o-3)

#### [Tensor Flow ](https://www.tensorflow.org/)

Tensor Flow is a machine library that was recently [open sourced by Google](http://googleresearch.blogspot.ca/2015/11/tensorflow-googles-latest-machine_9.html). It supports distribution on multiple CPUs and GPUs. Numerical computations are represented using so-called "data flow graphs". Therein, nodes represent mathematical operations while edges represent multidimensional data arrays (tensors) that are exchanged between the nodes.

- Languages: Python, C/C++ 
- License: [Apache 2](https://github.com/tensorflow/tensorflow/blob/master/LICENSE)
- Github: [https://github.com/tensorflow/tensorflow](https://github.com/tensorflow/tensorflow)

#### [Theano](http://www.deeplearning.net/software/theano/)

Theano is a python library focused on optimization and evaluation of mathematical expressions and targets on working efficiently with multidimensional arrays. It's a base for higher abstraction frameworks like [Lasagne](https://github.com/Lasagne/Lasagne) or [Keras](http://keras.io/). There are quite a few tutorials available showing how Theano supports different network types.

- Languages: Python
- License: [https://github.com/Theano/Theano/blob/master/doc/LICENSE.txt](https://github.com/Theano/Theano/blob/master/doc/LICENSE.txt)
- Github: [https://github.com/Theano/Theano/](https://github.com/Theano/Theano/)

#### [Torch](http://torch.ch/)

Torch is a scientific computing framework offering support for several machine learning algorithms. It's implemented in C and offers an interface via the LUA scripting language.

- Languages: C/C++, Lua
- License: [BSD License](https://en.wikipedia.org/wiki/BSD_License) 
- Github: [https://github.com/torch/torch7](https://github.com/torch/torch7)


### References
You can find more libraries here: [Data Science Central](http://www.datasciencecentral.com/profiles/blogs/here-are-15-libraries-in-various-languages-to-help-implement-your). And even more libraries here: [Teglor](http://www.teglor.com/b/deep-learning-libraries-language-cm569/).