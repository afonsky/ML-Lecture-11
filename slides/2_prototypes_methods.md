---
layout: chapter-title

# the image source
image: /midjourney_prototype_methods.jpg

# a custom class name to the content
class: my-cool-content-on-the-right
---

## Prototype Methods

<span style="color:DimGray; font-size: 11px; position:absolute; right:20px; bottom:20px;">Image credit: Midjourney<br> prompt: 'prototype method in computer science'
</span>

---
zoom: 0.93
---

# The Big Picture: How Do We Classify?

<div class="grid grid-cols-[5fr_3fr] gap-4">
<div>

* So far you've learned approaches that fit a **global model**:
* Logistic Regression, SVMs, Decision Trees, etc.
* Today: methods that classify based on **proximity** in feature space

<v-click>

* **Prototype methods**: summarize training data into representative points
* **kNN**: use the training data directly — no model fitting at all!
* Key idea:
> *"Sometimes the simplest method wins — kNN is hard to beat<br> when you have enough data and the right features"*<br>
> — Andrew Ng

</v-click>
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

* Core question: **how do we measure "closeness"?** — this will be our recurring theme today

<!--
Motivation slide — connect to what students already know.
Stress that kNN was briefly introduced in Lecture 2 (Linear Regression & kNN), now we go deep.
The key message: today's methods don't fit global models, they rely on local structure.
Ask students: "What's the simplest way you could classify a new point?" → Answer: look at its neighbors!
-->

---
zoom: 0.93
---

# Prototype Methods
### Summarize data into representative points

<div class="grid grid-cols-[5fr_3fr] gap-2">
<div>

* Given training set $\mathcal{T} := \{(x_i, g_i)\}_{i=1}^{N}$ with class labels $g_i \in \{1, \ldots, K\}$
* **Idea**: represent each class by a few **prototype** points in feature space
* Prototypes are *constructed*, typically **not** in training set
* Each prototype has a class label
* Classify new point → assign to class of **closest** prototype
* Prototype models are:
* Non-parametric — the "model" is the set of prototypes
* Local — decision depends on which prototype is closest
</div>
<div>
<br>
  <figure>
    <img src="/ESL_figure_13.1a.png" style="width: 350px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/">ESL Fig. 13.1 (top)</a>
    </figcaption>
  </figure>
</div>
</div>

* Decision boundary: the set of points equidistant from two prototypes of different classes

<!--
Explain the geometric intuition: decision boundary is a Voronoi tessellation.
Each prototype "owns" the region of space closest to it.
Key point: prototypes are CONSTRUCTED, not necessarily actual data points.
Contrast with kNN which will use ALL training points.
-->

---
zoom: 0.8
---

# k-Means: The Workhorse of Clustering

<div class="grid grid-cols-[1fr_1fr] gap-4">
<div>

* **k-Means Algorithm** (Josh Starmer: *"gloriously simple"*):
1. Pick $R$ random points as initial cluster centers
2. Assign each point to its $L_2$-closest centroid
3. Recompute centroids as **mean** of assigned points
4. Repeat steps 2-3 until convergence
* Minimizes **within-cluster sum of squares** (inertia)
* ⚠️ Sensitive to initialization → use **k-means++** or multiple restarts
</div>
<div>

* **k-Medoids** (PAM):
* Cluster centers **must be actual data points** (medoids)
* Works with **any** distance metric (not just Euclidean)
* More robust to outliers, easier to interpret
* **k-Medians**:
* Minimizes $L_1$ distance (sum of absolute deviations)
* Uses coordinate-wise median → more robust to outliers than k-Means

</div>
</div>

<br>

| | k-Means | k-Medoids | k-Medians |
|---|---|---|---|
| **Center type** | Mean (any point) | Actual data point | Coordinate-wise median |
| **Metric** | $L_2$ only | Any | $L_1$ |
| **Outlier robustness** | Low | High | Medium |

<!--
Spend 2 min on this recap — students know k-Means from unsupervised learning.
Emphasize k-means++ initialization (Arthur & Vassilvitskii 2007) — always use it in practice.
The comparison table is key: k-Medoids lets you use ANY distance metric.
Ask: "When would you prefer k-Medoids over k-Means?" → When features aren't numeric, or you need interpretability.
-->

---
zoom: 0.93
---

# k-Means for Classification

* For each class $k$: run k-Means within that class to find $R$ centroids → label them with class $k$
* Classify new test point → class of the **closest prototype**
<div class="grid grid-cols-[4fr_2fr]">
<div>

* **Problem**: prototypes are found **independently** per class
* Prototypes near class boundaries → misclassifications!
* Ex. [ESL Fig 13.1](https://hastie.su.domains/ElemStatLearn/):
* $\color{#56B4E9} x_1$ is among $\color{green} \mathrm{greens}$, but classified $\color{#56B4E9} \mathrm{blue}$
* b/c nearest prototype is $\color{#56B4E9} \mathrm{blue}$
 * $\color{#E69F00} x_2$ is among $\color{green} \mathrm{greens}$, but classified $\color{#E69F00} \mathrm{orange}$
* $\color{#E69F00} x_3$ is among $\color{#56B4E9} \mathrm{blues}$, but classified $\color{#E69F00} \mathrm{orange}$
* **Solution**: make prototypes boundary-aware → **LVQ**
</div>
<div>
  <figure>
    <img src="/ESL_figure_13.1a.png" style="width: 420px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/">ESL Fig. 13.1 (top)</a>
    </figcaption>
  </figure>
</div>
</div>

<!--
This is the KEY motivating example — walk through the figure carefully.
Point to x1, x2, x3 and show WHY they're misclassified: the prototypes were found independently within each class, ignoring the other classes.
This naturally motivates LVQ: we need prototypes that are SUPERVISED, aware of class boundaries.
-->

---
zoom: 0.93
---

# Learning Vector Quantization (LVQ)

<div class="grid grid-cols-[2fr_2fr]">
<div>
  <figure>
    <img src="/ESL_figure_13.1a.png" style="width: 340px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>k-Means: 5 prototypes & 3 classes
    </figcaption>
  </figure>
</div>
<div>
  <figure>
    <img src="/ESL_figure_13.1b.png" style="width: 340px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>LVQ: 5 prototypes & 3 classes
    </figcaption>
  </figure>
</div>
</div>

* Notice: LVQ prototypes move **away** from decision boundaries → better classification
* **Key insight**: training points **attract** same-class prototypes and **repel** other-class prototypes
* LVQ is **online** — processes one observation at a time (similar to SGD)

---
zoom: 0.95
---

# LVQ1 Algorithm

<div class="bg-orange-100">

1. **Initialize**: $R$ prototypes per class, $m_1(k), ..., m_R(k)$ for $k = 1:K$; learning rate $\epsilon > 0$
2. Sample random $(x_i, g_i)$ and find its **closest prototype** $m_j(k)$:
* $m_j(k) ~\leftarrow~ m_j(k) + \epsilon \big(x_i - m_j(k) \big) \cdot \begin{cases} ~~1~, & \mathrm{if} ~~g_i = k ~~\text{(correct class: pull closer)}\\ -1, & \mathrm{if} ~~g_i \neq k ~~\text{(wrong class: push away)} \end{cases}$
3. Decrease $\epsilon$ and repeat step 2
</div>

<br>

* **Intuition** (Raschka): LVQ is like "supervised k-Means" — it uses class labels to reshape the Voronoi regions

<v-click>

* Resembles SGD: one-at-a-time updates that gradually improve prototypes
* LVQ bridges unsupervised clustering and supervised classification

</v-click>

<!--
Walk through the algorithm step by step.
Draw an analogy to SGD: each step processes one random training example.
Key insight: the update rule is like gradient descent, but on prototype positions.
Correct class → prototype moves toward the point (attractive force).
Wrong class → prototype moves away (repulsive force).
Mention that learning rate schedule matters — too high → oscillation, too low → slow convergence.
-->
