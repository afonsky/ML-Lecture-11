---
layout: chapter-title

# the image source
image: /midjourney_prototype_methods.jpg

# a custom class name to the content
class: my-cool-content-on-the-right
---

## Prototype Methods

<span style="color:DimGray; font-size: 11px; position:absolute; right:20px; bottom:20px;">Image credit: Midjourney<br> prompt: ‘prototype method in computer science'
</span>

---
zoom: 0.93
---

# Prototype Methods
### Here we mix unsupervised and supervised methods

* Def.: $\mathcal{T} := \{(x_i, g_i\}_{1:N}$ taining observations with $g_i \in \{1, ..., K\}$ class labels, $x_i \in \mathbb{R}^p$
<div class="grid grid-cols-[5fr_3fr] gap-2">
<div>

* Prototype models:
	* non-parametric, no fixed number of parameters
		* harder to interpret I/O relation (varies locally)
			* vs. logistic regression,<br> where I/O relationship is global
  * effective black box prediction
  * represent $\mathcal{T}$ with **prototype** points in feature space $\mathbb{R}^p$, but (typically) **not in** $\mathcal{T}$
* Prototypes have class labels
</div>
<div>
	<br>
  <figure>
    <img src="/k_means_3_clusters.png" style="width: 420px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
	  <a href="https://rpubs.com/cyobero/k-means">https://rpubs.com/cyobero/k-means</a>
    </figcaption>
  </figure>
</div>
</div>

 * Test points are assigned to the "closest" prototype; Euclidean metric is "natural"

---
zoom: 0.86
---

# k-Means, k-Medians, k-Medoids

* **k-Means Algorithm**:
	* Initialize with $R$ random cluster centers (each centroid is a **mean** of points in a cluster)
	* Assign each point to the $L_2$-closest centroid 
		* This yields the smallest **intra-cluster variance** (sum of squared errors)
	* Recompute centroids and repeat till convergence
* **k-Medoids**:
	* Same as k-means, but allows **any** distance metric and cluster centers **must be** medoids
		* Def: **medoid** is a representative point of the sample and from the sample
		* Easier to interpret
* **k-Medians** (often confused with k-medoids!):
	* Same as k-means, but assigns each point to the $L_1$-closest centroid (minimizes $L_1$ within-cluster error)
		* Uses median of each attribute of points in a cluster.
			* I.e. $\mathrm{med}\big(\left[\begin{smallmatrix}0\\3\end{smallmatrix}\right], \left[\begin{smallmatrix}2\\3\end{smallmatrix}\right] \big) = \left[\begin{smallmatrix}0\\3\end{smallmatrix}\right]$
			* Note: $\left[\begin{smallmatrix}1\\3\end{smallmatrix}\right]$ is not in train set and even value $1$ is not among original values

---
zoom: 0.93
---

# k-Means for Classification of Labeled Observations

* For each class $k$:
	* Use $k$-Means to find $R$ centroids (i.e. prototypes) and label them with class $k$
* Classify new test point to the class of the closest prototype
* Decision boundary is equidistant between two neighboring prototypes (of differing classes)
<div class="grid grid-cols-[4fr_2fr]">
<div>

* Inter-class prototypes
	* near boundaries yield more misclassifications
	* are *constructed* independently
* Ex. [ESL Fig 13.1](https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=480)
	* $\color{#56B4E9} x_1$ is among $\color{green} \mathrm{greens}$, but is classified to $\color{#56B4E9} \mathrm{blue}$
		* b/c nearest prototype is $\color{#56B4E9} \mathrm{blue}$
 	* $\color{#E69F00} x_2$ is among $\color{green} \mathrm{greens}$, but is classified to $\color{#E69F00} \mathrm{orange}$ 
	* $\color{#E69F00} x_3$ is among $\color{#56B4E9} \mathrm{blues}$, but is classified to $\color{#E69F00} \mathrm{orange}$
</div>
<div>
  <figure>
    <img src="/ESL_figure_13.1a.png" style="width: 420px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=480">ESL Fig. 13.1 (top)</a>
    </figcaption>
  </figure>
</div>
</div>

---
zoom: 0.93
---

# Learning Vector Quantization (LVQ)

<div class="grid grid-cols-[2fr_2fr]">
<div>
  <figure>
    <img src="/ESL_figure_13.1a.png" style="width: 340px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>k-Means 5 prototypes & 3 classes
    </figcaption>
  </figure>
</div>
<div>
  <figure>
    <img src="/ESL_figure_13.1b.png" style="width: 340px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>LVQ 5 prototypes & 3 classes
    </figcaption>
  </figure>
</div>
</div>

* Prototypes are away from decision boundary: this improves classification near boundary
* Train points attract intra-class prototypes and repel inter-class prototypes
* LVQ is an **online** algorithm, i.e. observations are processed one at a time

---

# LVQ1 Algorithm

<div class="bg-orange-100">

1. Initialize:
	1. $R$ prototypes for each class, $m_1(k), ..., m_R(k)$ for $k = 1:K$
	2. Learning rate $\epsilon > 0$
2. Bootstrap $(x_i, g_i)$ and assign $x_i$ it to the **closest prototype** $m_j(k)$
	* Update prototype:<br> $m_j(k) ~\leftarrow~ m_j(k) + \epsilon \big(x_i - m_j(k) \big) \cdot \begin{cases} ~~1~, & \mathrm{if} ~~g_i = k\\ -1, & \mathrm{if} ~~g_i \neq k \end{cases}$
		* If true and predicted labels match, move the prototype closer to $x_i$,<br> otherwise, move it away from $x_i$
	* Reduce $\epsilon$
3. Repeat step 2
</div>