---
layout: post
title: Intro
keywords: machine learning
permalink: Introml.html
folder: ML
---
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script> 

### Intro
One of main application of machine learning is Classification. The classification is the process of  predicting the object category with given sample data. The classification is achieved either by constructing a model for each class (generative) or by finding the decision boundary between classes (discriminative) in the sample space ([Figure 1](#figure1)).  

<div style="text-align:center" markdown="1">
<a name="figure1"></a>![Figure 1](/images/ML.png){:width="500px"}
 </div>   
**Figure 1.** *The generative approach onthe left assigns the new sample, grey triangle, to the closest class model, while the discriminative classification, right side, used direct mapping by finding the boundaries between the classes C1, and C2. The red line is classifying plane.*

Red and blue markers in [Figure 1](#figure1) are the feature vectors. A feature vector is an n-dimensional vector of numerical features that represent some property of object. 

### Discriminative Classifiers
Assminug, features vector is represented by  $$\bf{x}$$, and  the  object classes is indicated by  $$ y=\{1,\dots,C\} $$. Then a maximising posterior probability (MAP) is used to assign each $$\bf{x}$$ to the $$\bf{y}$$ with maximum likelihood,&nbsp; $$ p(y \mid \bf{x}) $$. 

For the image, the features vector can be extracted from pixels value. So that, three channel image (RGB)  of size $$32\times32$$ have $$3072$$  dimension ($$32\times32\times3=3072$$). With $$ m $$ training example the features vectors and class labels can be shown by,  

\begin{equation}
X = 
\begin{bmatrix} \colon& \colon&\colon &\colon\\
				x^1& x^2&\cdots & x^m \\
				\colon& \colon&\colon &\colon
\end{bmatrix}
\end{equation}
\begin{equation}
Y=
\begin{bmatrix}y^1& y^2&\cdots & y^m \end{bmatrix}
\end{equation}  

where $$ x\in \Re^{n_x} $$, $$ X\in \Re^{(n_x,m)} $$, $$ y\in \Re $$ and $$ Y\in \Re^{(1,m)} $$.

[\\]:(genrative model, traning, lableing, test)



[\\]:#### [Decision trees]()
[\\]:#### [Random Forest]()
[\\]:#### [Boosted Trees]()
#### [Logistic Regression](Logistic.html)
[\\]:#### [Linear Regression]()
[\\]:#### [Nearest Neighbor]()
[\\]:#### [Support Vector Machine (SVM)]()
[\\]:#### [Neural Networks]()



