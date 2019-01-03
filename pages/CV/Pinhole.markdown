---
layout: post
title:  "Pinhole Camera Model"
keywords: opt, geo
tags: [Computer Vision, Theory]
summary: ""
permalink: Pinhole.html
---
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script> 


The simplest camera model is pinhole model which decribes the mathematical relationship of the projection of points in 3d-space onto a image plane. Let the centre of projection be the origin of a Euclidean coordinate system, and the plane &nbsp; $$ Z = f $$, which is called the *image plane* or *focal plane*.  Under pinhole camera model, a point in space with coordinates &nbsp; $$ (X, Y, Z)^T $$ is mapped to the point on the image plane &nbsp; $$ (\frac{fX}{Z}, \frac{fY}{Z}, f)^T $$  using triangles as shown in [Figure 1](#figure1).
Ignoring the final image coordinate, the central projection mapping from 3d world space to 2d image coordinates is,

$$ (X, Y, Z)^T\longrightarrow(\frac{fX}{Z},\frac{fY}{Z})^T 
    \label{mapping}
    \tag{1}
$$

<a name="figure1"></a> ![figure 1](/images/pn.jpg)
**Figure 1.** Pinhole camera geometry. The centre of projection is called the *camera centre* or the *optical centre*. The line from the camera centre perpendicular to the image plane is called the *principal axis* or *principal ray*. The point where the principal axis
meets the image plane is called the *principal point*. The plane through the camera
centre parallel to the image plane is called the *principal plane of the camera*. **C** is the camera centre and **p** the principal point. The camera	centre is here placed at the coordinate origin [[Hartley and Zisserman, 2003]](#ref1).


 Assuming the world and image points	are represented in [homogeneous] coordinates, then central projection can simply expressed as a linear mapping between their homogeneous coordinates in terms of matrix multiplication by,

$$ \begin{bmatrix}
		fX\\fY\\Z\\
		\end{bmatrix}=		
		\begin{bmatrix}
		f&0&0&0\\0&f&0&0\\0&0&1&0\\		
		\end{bmatrix}
		\begin{bmatrix}
		X_{cam}\\Y_{cam}\\Z_{cam}\\1\\
		\end{bmatrix}
    \label{eq:k1}
    \tag{2}
$$
<span style="font-size:larger;">**Principal point offset:**</span> In theory the origin of coordinates in the image plane  assumed to be at the principal point. This may not be true in practice, hence, the Eq. \eqref{eq:k1}&nbsp; is express as,

$$\begin{bmatrix}
	fX+ Zp_x\\fY+ Zp_y\\~Z\\		
	\end{bmatrix}=
	\begin{bmatrix}
	f_x&0&p_x&0\\0&f_y&p_y&0\\0&0&1&0\\		
	\end{bmatrix}
	\begin{bmatrix}
	X_{cam}\\Y_{cam}\\Z_{cam}\\1\\
	\end{bmatrix}
	\label{eq:k2}
  \tag{3}	
$$

First matrix in the right side of Eq. \eqref{eq:k2}&nbsp; called camera *calibration matrix*, usually expressed by $$ K $$. For added generality,  the calibration matrix
can be express as, 

$$
		K=
		\begin{bmatrix}
		\alpha_x&s&p_x\\0&\alpha_y&p_y\\0&0&1\\		
		\end{bmatrix}
		\label{eq:K}
    \tag{4}
$$
where&nbsp; $$s$$&nbsp; is referred to as the skew parameter which is zero for most  of the cameras. &nbsp; $$ f_x $$&nbsp; and&nbsp; $$ f_y $$&nbsp; where $$alpha_x = fm_x$$ and $$\alpha_y = fm_y$$ represent the focal length of the camera in terms of pixel dimensions in the *x-axis*  and the *y-axis* respectively, and&nbsp; $$ (p_x,p_y) $$&nbsp; is coordinate of the principal point.

<a name="figure2"></a> ![Figure 2](/images/rt.jpg)
<span style="font-size:12pt;">**Figure 2:** The Euclidean transformation between the world and camera coordinate frames [[Hartley and Zisserman, 2003]](#ref1).</span>


<span style="font-size:larger;">**Camera rotation and translation:**</span> The Subscripts  *cam* in above equations is to emphasize  that the points are represented as *camera coordinate frame*. In general, points in space is determined by the *world coordinate frame*.
The camera coordinate and world coordinate frames are related by *rotation* and *translation*. As it is shown in [Figure 2](#figure2), if&nbsp; $$ \textbf{X}=(X,Y,Z,1)^T $$&nbsp; is the coordinate of the point in the world coordinates, then $$ \textbf{X}_{cam} $$ is transformed by,

$$
	 \textbf{X}_{cam}=\big[R\quad t\big]\textbf{X}
	 \label{eq:world2cam}
    \tag{5}
$$

where **R** is&nbsp; $$ 3\times3 $$&nbsp; rotation matrix and&nbsp; **t** is&nbsp; $$ 3\times1 $$&nbsp; translation matrix.

Putting eveything together the formula for general mapping of pinhole camera in world coordinate frame **x** is defined by,

$$
	 \textbf{x}=\textbf{K}\big[\textbf{R}\quad \textbf{t}\big]\textbf{X}
	 \label{eq:finalpinhole}
   \tag{6}
$$

So that, the general pinhole camera matrix, **P**, can be represented by,

$$
	 \textbf{P}=\textbf{K}\big[\textbf{R}\quad \textbf{t}\big]=
	 \begin{bmatrix}
	 f_x&0&p_x\\0&f_y&p_y\\0&0&1\\		
	 \end{bmatrix}
	 \begin{bmatrix}
	 	 r_1&r_2&r_3&t_1\\r_4&r_5&r_6&t_2\\r_7&r_8&r_8&t_3\\		
	 \end{bmatrix}
	 \label{eq:p}
   \tag{7}
$$
and it has 9 degrees of freedom,

1. Three for &nbsp; **K**, namely,&nbsp; $$f$$, $$p_x$$, and  $$p_y$$
2. Three for rotation matrix **R** 
3. Three for translation **t**

**Internal camera parameters, K ,** show the internal orientation of the camera and it is fixed.
 
**External parameters, R and t** show camera orientation and position to a world coordinate system.


## References
<a name="ref1"></a> Richard Hartley and Andrew Zisserman. Multiple view geometry in computer
vision. Cambridge university press, 2003.

[homogeneous]: https://en.wikipedia.org/wiki/Homogeneous_coordinates
