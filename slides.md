---
# You can also start simply with 'default'
theme: seriph
addons:
  - "@twitwi/slidev-addon-ultracharger"
addonsConfig:
  ultracharger:
    inlineSvg:
      markersWorkaround: false
    disable:
      - metaFooter
      - tocFooter

background: /logo/ship2.jpg

# some information about your slides (markdown enabled)
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
# transition: slide-down
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true

title: Machine Learning
hideInToc: true
subtitle: Prototype Methods & kNN
date: 17/11/2025
venue: HSE
author: Alexey Boldyrev, Maksim Karpov
---

<br>
<br>
<br>
<br>

# <span style="font-size:32.0pt" v-html="$slidev.configs.title?.replaceAll(' ', '<br/>')"></span>
# <span style="font-size:32.0pt" v-html="$slidev.configs.subtitle?.replaceAll(' ', '<br/>')"></span>
# <span style="font-size:18.0pt" v-html="$slidev.configs.author?.replaceAll(' ', '<br/>')"></span>
# <span style="font-size:18.0pt" v-html="$slidev.configs.date?.replaceAll(' ', '<br/>')"></span>
<div>

<span style="color:#b3b3b3ff; font-size: 11px; line-height: 1.5em; float: right;">Image credit: ‘The Mayﬂower at Sea’<br> by Granville Perkins, 1876<br>
Wallach Division Picture Collection<br> The New York Public Library.
</span>
</div>

<div class="abs-tl mx-5 my-10">
  <img src="/logo/FCS_logo_full_L.svg" class="h-18">
</div>

<div class="abs-tl mx-5 my-30">
  <img src="/logo/DSBA_logo.png" class="h-28">
</div>

<div class="abs-tr mx-5 my-5">
  <img src="/logo/ICEF_logo.png" class="h-28">
</div>

<style>
  :deep(footer) { padding-bottom: 3em !important; }
</style>



<div class="abs-tl mx-5 my-10">
  <img src="/logo/FCS_logo_full_L.svg" class="h-18">
</div>

<div class="abs-tl mx-5 my-30">
  <img src="/logo/DSBA_logo.png" class="h-28">
</div>

<div class="abs-tr mx-5 my-5">
  <img src="/logo/ICEF_logo.png" class="h-28">
</div>

<style>
  :deep(footer) { padding-bottom: 3em !important; }
</style>

<!--
NB: This demo uses a custom syntax (using preparser extensions), with all the @@@@.
-->

---
src: ./slides/0_outline.md
---

---
src: ./slides/2_prototypes_methods.md
---

---
src: ./slides/3_gaussian_mixtures.md
---

---
src: ./slides/4_knn.md
---

---
src: ./slides/5_invariant_transformations.md
---

---
src: ./slides/0_end.md
---