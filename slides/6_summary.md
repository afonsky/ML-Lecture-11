---
zoom: 0.81
---

# Summary: When to Use What?

| Method | Best for | Strengths | Weaknesses |
|--------|----------|-----------|------------|
| **k-Means** | Simple clustering, spherical clusters | Fast, scalable | Hard assignment, spherical only |
| **LVQ** | Classification with prototypes | Supervised, boundary-aware | Sensitive to learning rate |
| **GMM** | Soft clustering, ellipsoidal clusters | Probabilistic, flexible shapes | Slow in high dim, needs init |
| **kNN** | Complex boundaries, moderate $p$ | No training, any boundary | Slow prediction, curse of dim |
| **Weighted kNN** | When neighbors vary in relevance | Smoother, less sensitive to $k$ | Still $O(Np)$ per query |
| **DANN** | High-dim with class structure | Adapts metric locally | Expensive per query |
| **Tangent dist.** | Known invariances (e.g. images) | Encodes domain knowledge | Domain-specific, slow |

<!--
Quick comparison table — don't read it line by line, let students absorb it.
Highlight the key trade-offs: simplicity vs flexibility, speed vs accuracy.
Main message: start simple (kNN), add complexity only when needed.
-->

---
zoom: 0.9
---

# Key Takeaways

<div class="grid grid-cols-[1fr_1fr] gap-8">
<div>

### Concepts
1. **Prototypes** compress training data into representatives; kNN uses all of it
2. **Soft vs. hard** clustering: GMM gives probabilities, k-Means gives labels
3. **kNN is non-parametric**: no assumptions about boundary shape
4. The **curse of dimensionality** makes all distance-based methods struggle in high $p$
5. **Feature engineering** and **distance metrics** matter enormously

<br>

> *"The methods you choose matter less than how well you understand your data."*
</div>
<div>

### Practical advice
1. **Always standardize** features before using distance-based methods
2. **Start simple**: try kNN with $k \approx \sqrt{N}$, tune with CV
3. Use **approximate NN** (KD-trees, FAISS) for large datasets
4. If clusters are non-spherical → GMM over k-Means
5. Encode **domain invariances** — via metric, augmentation, or architecture
6. **Plot your errors** — understand why points are misclassified before adding complexity
</div>
</div>


<!--
Final slide before end — leave students with practical takeaways.
Key messages:
1. Standardize features (can't stress this enough for distance-based methods)
2. Start with kNN as a baseline — it's surprisingly hard to beat
3. Use approximate NN for large datasets (mention scikit-learn's BallTree, KD-Tree)
4. Encode domain knowledge — whether in the distance metric, data augmentation, or model architecture
5. Always diagnose errors before adding complexity

Point to homework/lab where they'll implement kNN and compare with other methods.
-->
