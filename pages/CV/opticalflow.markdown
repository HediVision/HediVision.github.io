---
layout: page
title: Optical Flow
keywords: optical flow, Horn&Schunk, Horn-Schunk, Lucas–Kanade, LKT, Farneback, python, OpenCV
permalink: opticalflow.html
folder: computervision
---
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script> 


\\
A bit of theory always helps but if you want you can jump to my [Github](https://github.com/HediVision/OpticalFlow) page to see the step by step implementation of the optical flow models that I am discussing here.





### Introduction 


Motion information is one of the most valuable cues of the visual system and it is the foundation of many computer vision applications. These include visual navigation, segmentation, video compression, object tracking, and behavior analysis. Motion information in the time-ordered images sequence is extracted from estimating the displacements of intensity patterns which is known as Optical flow.

The basic hypothesis in estimating optical flow is brightness constancy which originated from the physiological definition of Perceptual Constancy.  The brightness constancy states that the intensity of moving pixels remains constant in a short duration. Let $$ I(x,y,t) $$ be the intensity for the pixel $$ (x,y) $$ at time $$ t $$ then,

$$
I(x,y,t)=I(x+\Delta x,y+\Delta y,t+\Delta t)
    \label{eq:k1}
    \tag{1}
$$

with applying Taylor expansion on the left side of Eq. \eqref{eq:k1}&nbsp;the optical flow constrain equation drives as,

$$
I_xu+I_yv+I_t=0
\label{eq:constrain}
\tag{2}
$$

where $$ I_x $$ and $$ I_y $$ is the spatial intensity gradient and $$ I_t $$ is temporal gradient and $$ (u,v) $$ is velocity vector. 


### Horn and Schunk

The brightness constancy constraint alone is inadequate to solve the ambiguity of motion flow. With assuming motion field varies smoothly over frames (smoothness constraint), Horn and Schunk proposed first variational optical flow estimation. Horn and Schunk formulated motion flow as a global energy function. This energy function should be minimized with respect to two error terms:

--- | --- | :----:
**1.** | **Data Error:** | $$ \varepsilon_{d}=\sum (u.I_x+v.I_y+I_t)^2 $$ 
**2.** | **Smoothness Error:** | $$ \varepsilon_{s}=\sum(u_{x+1,y}-u_{xy})^2+(u_{x,y+1}-u_{xy})^2+ (v_{x+1,y}-v_{xy})^2+(v_{x,y+1}-v_{xy})^2 $$


So that, the total error is equal to, 
		
$$
\varepsilon_{t}=\varepsilon_{d}+\lambda \varepsilon_{s}\\
\label{eq:e_total}
\tag{3}
$$
		
Minimise the Eq. \eqref{eq:e_total}&nbsp; and with some variational calculus knowledge, we can drive the two main optical flow equations as,

$$
\begin{array}{l}
u=u_{av}-I_x\frac{I_xu_{av}+I_yv_{av}+f_t}{\lambda+I_x^2+I_y^2}\\

v=v_{av}-I_y\frac{I_xu_{av}+I_yv_{av}+f_t}{\lambda+I_x^2+I_y^2}
\end{array}
\label{eq:hs_final}
\tag{4}
$$
		 
where $$ u_{av} $$ and $$ v_{av} $$ are the average flow of some neighbours around pixle $$(x,y)$$. The optimal $$(u,v)$$ is approximated by iterativly  estimate  $$u$$ and $$v$$ using Eq. \eqref{eq:hs_final}&nbsp;

---

**You can check my step by step implementation of Horn and Schunkon model in [here](https://github.com/HediVision/OpticalFlow/tree/master/HornSchunck).**

---

### Lucas–Kanade Method

Horn and Schunck assume the global smoothness constraint to solve the motion ambiguity, in contrast Lucas–Kanade  used local smoothness constraint. 
Lucas–Kanade assumed  the motion is constant  at some neighbourhood around pixles and solves the optical flow equations for all the pixels in that neighbourhood. Thus the motion vector $$ (u,v) $$ can be estimated by finding a solution  that minimizes the sum of squared errors,	

$$
   min\sum_{i=1}^{n}(I_{ix}u+I_{iy}v+I_t)
   \label{eq:klt}
\tag{5}
$$

$$n$$ is the number of pixel neighbourhoods. For $$ 3\times3 $$ neighbourhood $$n=9$$, thus we have $$9$$ equations and $$2$$ unknown $$ (u,v) $$ (over-determined system) and $$u$$ and $$v$$ is estimated by,



$$
		  \Big[\begin{array}{l}
		  u
		  \\
		  v
		  \end{array}\Big]=
		  \Bigg[\begin{array}{ll}
		  \sum_i I^2_{ix} & \sum_i I^2_{ix}I^2_{iy}
		  \\
		  \sum_i I^2_{ix}I^2_{iy} & \sum_i I^2_{iy}
		  \end{array}\Bigg]^{-1}
		  \Bigg[\begin{array}{l}
		  -\sum_i I^2_{ix}I^2_{it}
		  \\
		  -\sum_i I^2_{iy}I^2_{it}
		  \end{array}\Bigg]
		  \label{eq:e_terms}
\tag{6}
$$

---


**You can check my step by step implementation of Lucas–Kanade Method model in [here](https://github.com/HediVision/OpticalFlow/tree/master/LK).**

---

### Farneback

Farneback used second-degree polynomial expansion to calculate motion. Farnback model contains two main steps:

**1- Approximating the neighbourhood around each pixel as a quadric function using polynomial expansion**

**2- Use the expansion coefficients to estimate the displacement of pixels** 

Polynomial expansion approximating the neighbourhood around a pixel by a quadratic equation as,

$$
f(X)=X^TAX+b^T+c,
\label{eq:poly}
\tag{7}
$$

This approximation achieved by applying an operation called  **Normalized convolution** using basis of $$ \{1, x, y, x^2 y^2, xy\} $$ as:


$$
r=(B*W_aW_cB)^{-1}B*W_aW_cf
\label{eq:norm_conv}
\tag{8}
$$

Where $$B= \{1, x, y, x^2 y^2, xy\}$$ is the basis, $$a$$ is an applicability matrix, $$W_{a}$$ is diagonal of $$a$$,  $$c$$ is a certainty matrix and $$W_{c}$$ is diagonal of $$c$$ 

putting Eq. \eqref{eq:norm_conv}&nbsp; and Eq. \eqref{eq:poly}&nbsp; together we can represent the quadratic equation as a form of tensor by,

$$
f=\begin{bmatrix}x& y
\end{bmatrix} A
\begin{bmatrix}
x\\ 
y
\end{bmatrix}+
b^T
\begin{bmatrix}
x\\ 
y
\end{bmatrix}+c
$$

where

$$
X=\begin{bmatrix}
x\\ 
y
\end{bmatrix},
A=\begin{bmatrix}
r_4& \frac{r_6}{2}\\ 
\frac{r_6}{2}&r_5
\end{bmatrix}
b=\begin{bmatrix}
r_2\\ 
r_3
\end{bmatrix},
c=r_1
\label{eq:tensor}
\tag{9}
$$

where $$ r_1 $$ to $$ r_6 $$ are expansion coefficient. 


Based on brightness constancy,  the displacement vector $$ d $$ can be found by solving the following equation,


$$ 
f_2(X)=f_1(X-d)
$$ 


which result in:

$$
\begin{array}{l}
A_2=A_1\\
b_2=b_1-2A_1d\\
c_2=d^TA_1d-b_1^Td+c_1,
\end{array}
\label{eq:d}
\tag{10}
$$

so that, $$ d  $$ is then estimated by 

$$
d=-\frac{1}{2}A_1^{-1}(b_2-b_1).
\label{eq:d2}
\tag{11}
$$

and this result can improved iteratively by,

$$
\begin{array}{l}
\bar{b}_2=b_1-2A_1\bar{d}\\
d=-\frac{1}{2}A_1^{-1}(b_2+\bar{b}_2-b_1).
\end{array}
\label{eq:iterative}
\tag{12}
$$

---

**You can check my step by step implementation of Farneback  Method model in [here](https://github.com/HediVision/OpticalFlow/tree/master/Farneback).**

---




