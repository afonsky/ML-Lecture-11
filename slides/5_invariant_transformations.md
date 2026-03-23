---
layout: chapter-title

# the image source
image: /midjourney_invariant_transformations.jpg

# a custom class name to the content
class: my-cool-content-on-the-right
---

## Adaptive Metrics &<br>Invariant Transformations

<span style="color:DimGray; font-size: 11px; position:absolute; right:20px; bottom:20px;">Image credit: Midjourney<br> prompt: 'invariant transformations'
</span>

---
zoom: 0.9
---

# $k$NN in High Dimensions: The Curse of Dimensionality

<div class="grid grid-cols-[5fr_4fr] gap-6">
<div>

* In high dimensions, **all points become nearly equidistant**
	* kNN breaks down because "nearest" neighbor isn't much nearer than the farthest
* Consider unit hypercube $\big[-\frac{1}{2}, \frac{1}{2}\big]^p$:
* The radius $R$ needed to capture the nearest neighbor grows toward 0.5 as $p$ increases
* Neighborhoods become **huge** — covering most of the space
> *"In high dimensions, locality is an illusion — everything is far from everything else"*<br>
> — Abu-Mostafa
</div>
<div>
<br>
  <figure>
    <img src="/ESL_figure_13.12.png" style="width: 360px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="right"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/">ESL Fig. 13.12</a></div>
    </figcaption>
  </figure>
</div>
</div>

* **Practical consequence**: if you have many irrelevant features, kNN will perform poorly
* **Solutions**: feature selection, dimensionality reduction (PCA), or **adaptive metrics**

<!--
This is the hardest conceptual slide — slow down here.
Draw a 2D square on the board. A neighborhood of radius r covers a fraction of the space.
In 10D, to cover the same fraction, r must be almost as large as the side length.
Consequence: your "nearest" neighbor might actually be very far away.
Connect to what they know: PCA can reduce dimensions, feature selection removes noise features.
But sometimes we need smarter distance metrics → DANN.
-->

---

# A Case for Adaptive Metrics ([ESL Fig. 13.13](https://hastie.su.domains/ElemStatLearn/))

<div class="grid grid-cols-[5fr_2fr] gap-3">
<div>

* A **spherical** neighborhood misclassifies $x_0$ as red — but it's in the green region!
* Why? Euclidean distance treats all directions equally
	* But green points are clustered **above** $x_0$, not around it uniformly
* A **rectangular/ellipsoidal** neighborhood that stretches in the right direction classifies $x_0$ correctly as green
* **Goal**: adapt the metric to stretch the neighborhood where class probabilities are **stable**, and shrink it where they **change**
</div>
<div>
<br>
  <figure>
    <img src="/ESL_figure_13.13.png" style="width: 300px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="center"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/">ESL Fig. 13.13</a></div>
    </figcaption>
  </figure>
</div>
</div>

* Stretch orthogonal to the line joining class centroids; shrink along it

<!--
Use the figure: the spherical neighborhood includes red points that shouldn't be there.
The stretched (rectangular) neighborhood correctly captures only green points.
Intuition: stretch in directions where the class label doesn't change much, shrink where it changes rapidly.
This is like learning a Mahalanobis distance LOCALLY around each query point.
-->

---
zoom: 0.93
---

# Discriminant Adaptive NN (DANN)

<div class="grid grid-cols-[5fr_2fr] gap-3">
<div>

* DANN adapts the distance metric **locally** around each query point:
	1. Find $N_0$ (e.g. 50) neighbors of $x_0$
	2. Compute local inter-class and intra-class structure
	3. Stretch the metric in directions where classes **overlap**
* Distance metric: $D(x, \nu) = (x - \nu)^T \Sigma (x - \nu)$, where:
	* $W$ = pooled **within-class covariance** (how spread out each class is)
	* $B$ = **between-class covariance** (how separated class means are)
	* $\Sigma$ combines $W$ and $B$ to emphasize discriminative directions
* **Effect**: neighborhood becomes an ellipsoid aligned with the class boundary
</div>
<div>
<br>
  <figure>
    <img src="/ESL_figure_13.14.png" style="width: 300px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="center"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/">ESL Fig. 13.14</a></div>
    </figcaption>
  </figure>
</div>
</div>

* Spheres the data → then stretches in the zero-eigenvalue direction of between-class covariance

---
zoom: 0.95
---

# Invariant Transformations for Image Classification

<div class="grid grid-cols-[6fr_4fr] gap-2">
<div>

* A digit classifier should be **invariant** to:
	1. Small rotations (clockwise/anticlockwise)
	2. Horizontal/vertical translation (shifts)
	3. Horizontal/vertical scaling
	4. Shear deformation
	5. Stroke thickness
* **Problem**: these transforms produce images that are **far apart** in pixel space $\R^{256}$
	* A slightly rotated "3" looks identical to us, but the pixel vectors are very different

<v-click>

* LeCun: *"The key insight of feature engineering is building invariance to irrelevant transformations"*

</v-click>
</div>

<div>

<div align="center">16 x 16 = 256 pixels, gray scale</div>
<br>
  <figure>
    <img src="/ESL_figure_13.9.png" style="width: 600px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="center"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/">ESL Fig. 13.9</a></div>
    </figcaption>
  </figure>

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
zoom: 0.85
---

# Tangent Distance: Making kNN Invariant

<div class="grid grid-cols-[9fr_5fr] gap-2">
<div>
  <figure>
    <img src="/ESL_figure_13.10a.png" style="width: 360px !important">
  </figure>
<br>

* As we rotate image $x$ by angle $\alpha$,<br> it traces a curve (**invariance manifold**) in $\R^{256}$
* **Invariant metric**: shortest distance between two manifolds
	* Computationally very expensive!
* **Tangent distance**: approximate by tangent lines at each image
	* Tangent line = direction of small rotation at the original image
	* Distance = shortest distance between two tangent lines
* To classify with 1-NN:
	* Compute tangent line $t_0$ for query image $x_0$
	* Find training image $x_i$ with smallest tangent distance to $x_0$
	* Predict: $\hat{g}_0 := g_i$
</div>
<div>
<br>
  <figure>
    <img src="/ESL_figure_13.11.png" style="width: 380px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="center"><br>Images source:
      <a href="https://hastie.su.domains/ElemStatLearn/">ESL Fig. 13.10 (left) and 13.11 (right)</a></div>
    </figcaption>
  </figure>
</div>
</div>

---
zoom: 0.75
---

# Tangent Distance: Results on Digit Recognition

* Using all 7 transforms → compute **7D tangent hyperplanes** and distances between them

| Method                    | Error rate |
|---------------------------|------------|
| Neural Network            | 4.9%       |
| 1-NN with Euclidean distance | 5.5%   |
| **1-NN with tangent distance**  | **2.6%** |

<br>

<div class="grid grid-cols-[1fr_1fr] gap-6">
<div>

### Why this matters
* Tangent distance **halved** the error rate vs. Euclidean 1-NN
* Even beat a neural network! (on this pre-deep-learning benchmark)
* **Takeaway**: never underestimate **domain-specific feature engineering**
	* Always investigate **why** observations are misclassified
</div>
<div>

### Modern perspective (LeCun, Bengio)
* Today, **data augmentation** achieves similar goals:
	* Apply random rotations, shifts, scaling to training data
	* Let the neural network learn invariances from augmented data
* **Convolutional neural networks** build translation invariance into their architecture
* But the **principle** remains: encode known invariances, either in the metric or in the model
</div>
</div>

<!--
This is the historical perspective slide — connects classical ML to deep learning.
Tangent distance is elegant but complex. Data augmentation achieves the same goal more simply.
CNNs build translation invariance into the architecture (weight sharing, pooling).
The PRINCIPLE is what matters: if you know your problem has symmetries, exploit them.
This is still relevant today: equivariant neural networks, group convolutions, etc.
End message: domain knowledge always helps, whether encoded in metrics, augmentation, or architecture.
-->
