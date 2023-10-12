---
background: /midjourney_curse_of_dimensionality.jpg
layout: cover
hideInToc: true
---
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

# Dimensionality Reduction

<div>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<span style="color:gray; font-size: 11px; float: right;">Image credit: Midjourney.<br> Prompt: ‘curse of dimensionality'
</span>
</div>
---

# Dimensionality Reduction

* Original predictors $X_{p \times 1}$ are transformed into **new predictors** $Z_{M \times 1}$, where $M < p$
* **Linear transformation matrix** $\phi_{p \times M}$ produces $M$ linear transformations from $X_{1:p}$ <br>
$Z := X^{\prime} \phi = [X_1, ..., X_p] {\scriptsize \begin{bmatrix}\phi_{11} & \ldots & \phi_{1M} \\ \vdots & \ddots & \vdots \\ \phi_{p1} & \ldots & \phi_{pM}\end{bmatrix}}$ <br>
$Z_m := \sum\limits_{j=1}^p \phi_{jm} X_j = X^{\prime} \phi_{\cdot, m}$, where $\phi_{\cdot, m} := [\phi_{11}, ..., \phi_{p1}]^{\prime}$
* So, we can achieve $M < n < p$ to be able to estimate least squares coefficients in <br> $\hat{y}_i := \hat{\theta}_0 + \hat{\theta}_1 z_1 + ... + \hat{\theta}_M z_M$
* Estimation of $\beta_{1:p}$ is equivalent to the estimation of $\theta_{1:M}$ for the given $\phi$ matrix:
$$\sum\limits_{m=1}^M \theta_m z_{im} = \sum\limits_{m=1}^M \theta_m \sum\limits_{j=1}^p \phi_{im} x_{ij} = \sum\limits_{m=1}^M \sum\limits_{j=1}^p \theta_m \phi_{jm} x_{ij} = \sum\limits_{j=1}^p \sum\limits_{m=1}^M \theta_m \phi_{jm} x_{ij} = \sum\limits_{j=1}^p \beta{j} x_{ij}$$

---

# Dimensionality Reduction: PCA on Advertising

* For $100$ cities we have the following predictors: Ad Spending ``ad`` & Population ``pop``
* We wish to **decorrelate** the predictors via PCA: <br>
$Z_1 :=
\color{grey}\underbrace{\color{#006}{0.839}}_{\phi_{11}}
\color{#006}{~\cdot~}
\color{grey}\underbrace{\color{#006}{(\mathrm{pop} - \overline{\mathrm{pop}})}}_{\mathrm{residual}}
\color{#006}{~+~}
\color{grey}\underbrace{\color{#006}{0.544}}_{\phi_{21}}
\color{#006}{~\cdot~}
\color{grey}\underbrace{\color{#006}{(\mathrm{ad} - \overline{\mathrm{ad}})}}_{\mathrm{residual}}
\color{#006}{~=~}
\color{grey}\underbrace{\color{#006}{[0.839, 0.544]}}_{\mathrm{loadings}}
\color{#006}{~\cdot~} [r_{\mathrm{pop}}, r_{\mathrm{ad}}]$
	* We apply loadings to every observation to compute their first PC, $[z_{i1}]_{1:n}$
	* Loadings help us "move" points to the new position (or rotate coordinate system)
	* $\mathbb{V}Z_1$ is maximal
		* We note that ``pop`` is slightly more important than ``ad`` ($0.839 > 0.544$)
* If $\mathbb{V}Z_1 >> \mathbb{V}Z_2$, then we drop $Z_2$ and build <br> $Y | Z_1 = \theta_0 + \theta_1 Z_1 + \epsilon$
* PC variances are estimated by **eigenvalues**

---

# Dimensionality Reduction: PCA
### Principal Components Analysis (PCA)

<br>
<figure>
  <img src="/ISLP_figure_6.14.svg" style="width: 530px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 10px; left: 400px;">Image source:
    <a href="https://hastie.su.domains/ISLR2/ISLP_website.pdf#page=262">ISLP Fig. 6.14</a>
  </figcaption>
</figure>
<br>

## Don't forget to standartize variables before PCA!

---

# Dimensionality Reduction: Choosing # of PCs

* We prefer the number of PCs that capture **most** variability of all of the predictors $X_i$
	* The uncaptured variability is assumed to be rising from noise
* Scree or Elbow plot are often used
	* Look for the biggest drop in proportion of variance explained
		* Or for biggest rise in cumulative proportion of variance explained
* This compression works best for linearly related predictors

<div class="grid grid-cols-[7fr,8fr] gap-1">
<div>

* You can "compress" thousands of predictors to a dozen with little "information loss"
* For non-linearly related predictors, the scree plot may not have an "elbow"
</div>
<div>
<figure>
  <img src="/ISLP_figure_12.3.svg" style="width: 430px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 10px; left: 230px;">Image source:
    <a href="https://hastie.su.domains/ISLR2/ISLP_website.pdf#page=518">ISLP Fig. 12.3</a>
  </figcaption>
</figure>
</div>
</div>

---

# Dimensionality Reduction: Partial Least Squares

* In PCA, an unsupervised method, the response is not used
	* So, we can't guarantee that the PCs will perform better in predicting response
* PLS finds new features that best explain the response, $y$
	* Predictors most correlated to response get greatest weight
* Standardize $X_i$
* PLS direction $Z_1$ is determined by the coefficient $\beta_1$ from regresion $Y \sim X_1$
* PLS direction $Z_2$
	* Regress each remaining variable on $Z_1$ and consider just the residuals, $r_2$
		* They contain most of the remaining information
	* Then again, determine the direction of $Z_2$ from regression $r_2 \sim X_1$