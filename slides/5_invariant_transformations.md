---
layout: chapter-title

# the image source
image: /midjourney_invariant_transformations.jpg

# a custom class name to the content
class: my-cool-content-on-the-right
---

## Invariant Transformations

<span style="color:DimGray; font-size: 11px; position:absolute; right:20px; bottom:20px;">Image credit: Midjourney<br> prompt: ‘invariant transformations'
</span>

---

# Invariant Transformations

<div class="grid grid-cols-[6fr_4fr] gap-2">
<div>

* Digit classifier should ignore these transforms:
	1. Small clockwise/anticlockwise rotations
		* Large rotations work for digits other than 6-9
	2. Horizontal/vertical translation (i.e. shifts)
	3. Horizontal/vertical scaling
	4. Shear
	5. Character thickness
* However, these transforms produce digit vectors, which are far apart in $\R^{256}$

<br>
<figure>
	<img src="/ESL_figure_13.10a.png" style="width: 300px !important">
	<figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="center">Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=492">ESL Fig. 13.10 (top)</a></div>
    </figcaption>
</figure>
</div>

<div>

<div align="center">16 x 16 = 256 pixels, gray scale</div>
<br>
  <figure>
    <img src="/ESL_figure_13.9.png" style="width: 600px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="center"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=491">ESL Fig. 13.9</a></div>
    </figcaption>
  </figure>

<br>
<br>
<div class="grid grid-cols-[1fr_1fr]">
<div>
<br>
<img src="/shear_1.png" style="width: 160px !important">
</div>
<div>
<br>
<img src="/shear_2.png" style="width: 220px !important">
</div>
</div>
  <figure>
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="center"><br>Image source:
      <a href="https://en.wikipedia.org/wiki/Shear_force">https://en.wikipedia.org/wiki/Shear_force</a></div>
    </figcaption>
  </figure>
</div>
</div>

---
zoom: 0.95
---

# Invariance Manifolds

<div class="grid grid-cols-[3fr_2fr] gap-5">
<div>

* As we rotate the image by angle $\alpha$, we generate a 1D trajectory curve (**invariance manifold**) in $\R^{256}$ pixel space
* Let $x_1, x_2 \in \R^{256}$ be digit pixel vectors<br> with invariance manifolds $m_1, m_2$
* **Invariant metric** is the shortest Euclidean distance b/w curves $m_1, m_2$
* Problems:
	* It’s very computation intensive
	* It works better with longer curves, i.e. large transforms, but large transforms cause problems (e.g. 6 vs 9)
</div>
<div>
<br>
  <figure>
    <img src="/ESL_figure_13.10.png" style="width: 300px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="center"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=492">ESL Fig. 13.10</a></div>
    </figcaption>
  </figure>
</div>
</div>

---

# Tangent Distance

<div class="grid grid-cols-[9fr_5fr] gap-2">
<div>

* **Invariant tangent line** $\mathcal{t}_i$ estimates the direction vector from **small** rotations of the image $x_i$
	* Solves both problems of invariant metric
	* This is a tangent line at the point of original image
* **Tangent distance** $\mathcal{t}_{12}$ is the shortest distance between tangent lines $\mathcal{t}_1$, $\mathcal{t}_2$
* To apply 1-NN:
	* Given a query image $x_0$ compute its $\mathcal{t}_0$
	* Find training image $x_i$ with smallest $\mathcal{t}_{i0}$
	* $\hat{g}_0 := g_{\hat{\iota}}$, where $\iota := \underset{i}{\mathrm{argmin}} t_{i0}$
where $\hat{g}_0$ is the predicted class of $x_0$
</div>
<div>
<br>
  <figure>
    <img src="/ESL_figure_13.11.png" style="width: 360px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="center"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=493">ESL Fig. 13.11</a></div>
    </figcaption>
  </figure>
</div>
</div>

---
zoom: 0.88
---

# Back to 7 Transformations

* Transforms: rotations, 2x translations, 2x scaling, sheer, character thickness
* Each adds one more dimension to the curve in 
* So, we compute **7D tangent hyperplanes** and distances among their tangent hyperplanes
* Takeaway:
	* Never underestimate **feature engineering**
	* Always investigate:
		* **Why** the observations are misclassified and 
		* **How** you can express the distinctive features of misclassified observations

| Method                    | Error rate |
|---------------------------|------------|
| Neural-Net                | 0.049      |
| 1-NN / Euclidean distance | 0.055      |
| 1-NN / tangent distance   | 0.026      |


---

# $k$NN in High Dimensions (HD)

* Curse of dimensionality: in HD, points are spread out and are “near” the boundaries

	* $k$NN performance degrades

<div class="grid grid-cols-[2fr_2fr] gap-8">
<div>

* Consider unit cube in $\R^p$, $\big[-\frac{1}{2}, \frac{1}{2}\big]^p$

* $R$ = radius of 1-NN at $(0, 0)$,<br> which approaches 0.5 as $p$ grows

	* Median of $R$ is computed over different training sample sizes
</div>
<div>
<br>
  <figure>
    <img src="/ESL_figure_13.12.png" style="width: 360px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="right"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=495">ESL Fig. 13.12</a></div>
    </figcaption>
  </figure>
</div>
</div>

---

# A Case for Adaptive Metric (Ex. ESL Fig. 13.13)

* A spherical neighborhood misclassifies $x_0$ as red, but it’s in green region
<div class="grid grid-cols-[5fr_2fr] gap-3">
<div>

* b/c Euclidean metric assumes<br> **uniform distribution** of points in the disk
* but green points are clustered at the top
* A rectangular neighborhood classifies $x_0$ correctly as green
* Goal: adapt a metric to stretch the neighborhood in directions where class probabilities are stable
* How:
  * For a query point $x_0$ find $N_0$ (say 50) neighbors
  * Distribution of neighbors determines the metric
</div>
<div>
<br>
  <figure>
    <img src="/ESL_figure_13.13.png" style="width: 300px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="center"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=495">ESL Fig. 13.13</a></div>
    </figcaption>
  </figure>
</div>
</div>

* Stretch the neighborhood in the direction **orthogonal to the line** $\ell$ joining the class centroids

---

# Discriminant Adaptive Nearest-Neighbor (DANN)

<div class="grid grid-cols-[5fr_2fr] gap-3">
<div>

* DANN metric is $D (x, \nu) = (x - \nu)^{\prime} \Sigma (x - \nu)$,<br> where:
	* $\footnotesize \Sigma := W^{-\frac{1}{2}} \big[W^{-\frac{1}{2}} B W^{-\frac{1}{2}} + \epsilon I \big] W^{-\frac{1}{2}} = W^{-\frac{1}{2}} \big[B^{\ast} + \epsilon I\big] W^{-\frac{1}{2}}$
	* $W := \sum\limits_{k=1}^K \pi_k W_k$ is the pooled **intra-class covariance**
 	* $B = \sum_k \pi_k (\bar{x}_k - \bar{x}) (\bar{x}_k - \bar{x})^{\prime}$ is **inter-class covariance**
 	* $\epsilon$ rounds the rectangular neighborhood to an ellipsoid to avoid use of points in corners of a rectangle
		* $\epsilon = 1$ works well
</div>
<div>
<br>
  <figure>
    <img src="/ESL_figure_13.14.png" style="width: 300px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="center"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=497">ESL Fig. 13.14</a></div>
    </figcaption>
  </figure>
</div>
</div>

* The formula:
  * Spheres the data (pulls distant points closer to $x_0$)
  * Then stretches the neighborhood in the zero-eigenvalue direction of $B^{\ast}$