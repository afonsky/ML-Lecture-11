---
layout: chapter-title

# the image source
image: /midjourney_knn.jpg

# a custom class name to the content
class: my-cool-content-on-the-right
---

## k-Nearest Neighbors

<span style="color:DimGray; font-size: 11px; position:absolute; right:20px; bottom:20px;">Image credit: Midjourney<br> prompt: ‘k-Nearest Neighbors'
</span>

---

# k-Nearest Neighbor (kNN) Classifiers
<style>
.slidev-layout {
  font-size: 1.2em
}
</style>

* All train points are used in testing
	* There is no reduction in train set
* In $1$-NN, every point is a prototype
* $k$NN algorithm
	1. Standardize features as $x_i \leftarrow \frac{x_i - \bar{x}}{\sigma_{x_i}}$ (to have $0$ mean, unit variance)	
		* Otherwise, **smaller scale** features are **dominate** the neighborhoods
		* Typically, any $L_2$ distance-based algorithm can benefit from such feature normalization
	2. Classify the query point $x_0$, to the **mode (majority)** of labels of $k$ closest training points 
* $k$NN performs well with 
	* "irregular" (complex) decision boundaries
	* many prototypes (i.e. many clusters)
* We use Cross Validation (CV) to choose $k$

---

# Comparative Study

* K-Means vs LVQ vs $k$NN on 2 simulated problems:
  * All have $X_1, ..., X_{10} ~\overset{\mathrm{iid}}{\sim}~ \mathcal{U}(0,1)$

<div class="grid grid-cols-[4fr_5fr] gap-8">
<div>
<br>

* **Easy**: $Y := I (X_1 > 0.5)$
	* 9 features are ignored (just noise in 9 dimensions)

<v-plotly style="width: 350px; height: 200px !important"
:data="[{
x: Array.from({length: 15}, () => Math.random()*0.5),
y: Array.from({length: 15}, () => Math.random()*0),
type: 'scatter',
mode: 'markers',
marker: {color: 'blue', opacity: 0.9},
showlegend: false
},
{
x: Array.from({length: 15}, () => Math.random()*0.5+0.5),
y: Array.from({length: 15}, () => Math.random()*0),
type: 'scatter',
mode: 'markers',
marker: {color: 'orange', opacity: 0.9},
showlegend: false
}]"
:layout="{
xaxis: {zeroline: false},
yaxis: {showticklabels: false, showgrid: false},
margin: {l: 10, r:10, pad: 1}
}"
:config="{displayModeBar: false}"
:options="{}"/>

</div>
<div>

  * **Hard**: $Y := I \big(\mathrm{sgn}\big[\prod\limits_{j=1}^3 (X_j - 0.5)\big] > 0\big)$
    * 7 features are ignored

<v-plotly style="height: 250px; position: relative"
:data="[{
x: Array.from({length: 999}, () => Math.random()*0.5),
y: Array.from({length: 999}, () => Math.random()*0.5),
z: Array.from({length: 999}, () => Math.random()*0.5),
type: 'scatter3d',
mode: 'markers',
marker: {color: 'blue', size: 4, opacity: 0.7},
showlegend: false
},
{
x: Array.from({length: 999}, () => Math.random()*0.5),
y: Array.from({length: 999}, () => Math.random()*0.6+0.5),
z: Array.from({length: 999}, () => Math.random()*0.5),
type: 'scatter3d',
mode: 'markers',
marker: {color: 'orange', size: 4, opacity: 0.7},
showlegend: false
},
{
x: Array.from({length: 999}, () => Math.random()*0.6+0.5),
y: Array.from({length: 999}, () => Math.random()*0.6+0.5),
z: Array.from({length: 999}, () => Math.random()*0.5),
type: 'scatter3d',
mode: 'markers',
marker: {color: 'blue', size: 4, opacity: 0.7},
showlegend: false
},
{
x: Array.from({length: 999}, () => Math.random()*0.6+0.5),
y: Array.from({length: 999}, () => Math.random()*0.5),
z: Array.from({length: 999}, () => Math.random()*0.5),
type: 'scatter3d',
mode: 'markers',
marker: {color: 'orange', size: 4, opacity: 0.7},
showlegend: false
},
{
x: Array.from({length: 999}, () => Math.random()*0.5),
y: Array.from({length: 999}, () => Math.random()*0.5),
z: Array.from({length: 999}, () => Math.random()*0.6+0.5),
type: 'scatter3d',
mode: 'markers',
marker: {color: 'orange', size: 4, opacity: 0.7},
showlegend: false
},
{
x: Array.from({length: 999}, () => Math.random()*0.5),
y: Array.from({length: 999}, () => Math.random()*0.6+0.5),
z: Array.from({length: 999}, () => Math.random()*0.6+0.5),
type: 'scatter3d',
mode: 'markers',
marker: {color: 'blue', size: 4, opacity: 0.7},
showlegend: false
},
{
x: Array.from({length: 999}, () => Math.random()*0.6+0.5),
y: Array.from({length: 999}, () => Math.random()*0.6+0.5),
z: Array.from({length: 999}, () => Math.random()*0.6+0.5),
type: 'scatter3d',
mode: 'markers',
marker: {color: 'orange', size: 4, opacity: 0.7},
showlegend: false
},
{
x: Array.from({length: 999}, () => Math.random()*0.6+0.5),
y: Array.from({length: 999}, () => Math.random()*0.5),
z: Array.from({length: 999}, () => Math.random()*0.6+0.5),
type: 'scatter3d',
mode: 'markers',
marker: {color: 'blue', size: 4, opacity: 0.7},
showlegend: false
}]"
:layout="{
   scene: {camera: {eye: {x: 1.75, y: -1.25, z:1.05}},
            xaxis: {title: 'x<sub>1</sub>', range: [0.01,1]},
            yaxis: {title: 'x<sub>2</sub>', range: [0.,1]},
            zaxis: {title: 'x<sub>3</sub>', range: [0.,1]}},
   margin: {l: 20, r:20, b:2, t:1, pad: 5}
}"
:config="{displayModeBar: false}"
:options="{}"/>
</div>
</div>

---

# Comparative Study: Misclassification Error

<div class="grid grid-cols-[10fr_1fr_11fr]">
<div>
<br>

* $k$NN:
	* outperforms on difficult problem
	* underperforms on easy problem
* K-Means and LVQ perform similarly
</div>
<div>
</div>
<div>
  <figure>
    <img src="/ESL_figure_13.5.png" style="width: 420px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br><div align="center">Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=488">ESL Fig. 13.5</a></div>
    </figcaption>
  </figure>
</div>
</div>

---

# Ex: $k$NN for STATLOG Project

<div class="grid grid-cols-[11fr_9fr] gap-4">
<div>

* **Goal**: classify pixels of aerial images into 7 agricultural land use classes: cotton, vegetation stubble, mix, red soil, gray soil, damp gray soil, very damp gray soil<br>
  <figure>
    <img src="/ESL_figure_13.6.png" style="width: 380px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="left">Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=489">ESL Fig. 13.6</a></div>
    </figcaption>
  </figure>
</div>
<div>
<div align="center"><img src="/adjacent_pixels.png" style="width: 60px; position: relative; top: -70px;"></div>

* Each $x_i \in \R^{36}$ is built with 9 adjacent pixels from 4 spectral images
* $5$-NN resulted in 9.5% error
	* Better than many classifiers:
  <figure>
    <img src="/ESL_figure_13.8.png" style="width: 300px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><div align="right">Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=491">ESL Fig. 13.8</a></div>
    </figcaption>
  </figure>
</div>
</div>