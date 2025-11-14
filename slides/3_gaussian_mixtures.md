---
layout: chapter-title

# the image source
image: /midjourney_gaussian_mixtures.jpg

# a custom class name to the content
class: my-cool-content-on-the-right
---

## Gaussian Mixtures

<span style="color:DimGray; font-size: 11px; position:absolute; right:20px; bottom:20px;">Image credit: Midjourney<br> prompt: ‘gaussian mixtures'
</span>

---

# Gaussian Distribution

<div class="grid grid-cols-[2fr_2fr]">
<div>
  <figure>
    <img src="/640px-Normal_Distribution_PDF.svg.png" style="width: 340px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
    <a href="https://en.wikipedia.org/wiki/Normal_distribution">https://en.wikipedia.org/wiki/Normal_distribution</a>
    </figcaption>
  </figure>
</div>
<div>
  <figure>
    <img src="/1152px-Multivariate_Gaussian.png" style="width: 390px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
    <a href="https://en.wikipedia.org/wiki/Multivariate_normal_distribution">https://en.wikipedia.org/wiki/Multivariate_normal_distribution</a>
    </figcaption>
  </figure>
</div>
</div>
<br>

* For $d$ dimensions, the Gaussian distribution of a vector $x = (x_1, x_2, ..., x_d)^T$ is:<br>
$N(x | \mu, \Sigma) := \frac{1}{(2\pi)^{d/2} \sqrt{|\Sigma|}}\exp(-\frac{1}{2}(x-\mu)^T \Sigma^{-1}(x-\mu))$

---

# Mixture Models

* Formally a Mixture Model is the weighted sum of a number of pdfs where the weights are determined by a distribution, $\pi$
<br> $p(x) = \pi_0 f_0(x) + \pi_1 f_1(x) + ... + \pi_k f_k(x) = \sum\limits_{i=0}^k \pi_i f_i$,<br> where $\sum\limits_{i=0}^k \pi_i = 1$
<br>
<br>
<figure>
  <img src="/gm_1.png" style="width: 490px !important">
  <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
  <a href="https://nakulgopalan.github.io/cs4641/course/20-gaussian-mixture-model.pdf">https://nakulgopalan.github.io/cs4641/course/20-gaussian-mixture-model.pdf</a>
  </figcaption>
</figure>

---

# Mixture Models

<div class="grid grid-cols-[2fr_2fr]">
<div>
<figure>
  <img src="/gm_2.png" style="width: 320px !important">
  <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
  <a href="https://nakulgopalan.github.io/cs4641/course/20-gaussian-mixture-model.pdf">https://nakulgopalan.github.io/cs4641/course/20-gaussian-mixture-model.pdf</a>
  </figcaption>
</figure>
</div>
<div>

$p(x) = \pi_0 f_0(x) + \pi_1 f_1(x) + \pi_2 f_2(x)$

* Mixture model creates a new pdf for us to generate random variables. It is
a generative model.
* It clusters different components using a Gaussian distribution.
  * So it provides us the inferring opportunity. Soft assignment!
</div>
</div>

---

# Gaussian Mixtures Model (GMM)

* Each cluster $r$ is represented as $\mathcal{N}\big(\mu_{r\color{grey}{, p \times 1}}, \Sigma_{r\color{grey}{, p \times p}}\big)$

* We use **expectation maximization (EM) algorithm** to estimate $R$ Gaussian PDFs:
	* **E-step**: assign **responsibility** $p$ to each $x_i$
		* $p^{\prime} = [p_1, ..., p_R]$ are probability weights with $p_r := \mathbb{P}[g_i = r|\mathrm{all~params}]$<br> that $x_i$ is in each of $R$ clusters
	* **M-step**: update $R$ Gaussian parameters $\big(\mu_r, \Sigma_r \big)$ for the current responsibilities
		* each observation contributes to weighted means and covariances for each cluster
* GMM is **a soft clustering** prototype model
	* Recall: K-Means is **hard clustering**

---
zoom: 0.93
---

# Example: GMM vs K-Means ([ESL Fig. 13.2](https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=483))

<div class="grid grid-cols-[2fr_2fr]">
<div>
  <figure>
    <img src="/ESL_figure_13.2a.png" style="width: 340px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>K-Means 5 prototypes & 2 classes
    </figcaption>
  </figure>
</div>
<div>
  <figure>
    <img src="/ESL_figure_13.2b.png" style="width: 340px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>GMM 5 prototypes & 2 classes
    </figcaption>
  </figure>
</div>
</div>
<br>

* GMM has smoother boundaries vs K-Means has piecewise linear decision boundary
* GMM derives a better decision boundary. Both models estimate one wrong prototype	
