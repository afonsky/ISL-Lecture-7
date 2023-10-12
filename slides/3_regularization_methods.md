---
background: /midjourney_lasso.jpg
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

# Regularization Methods

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
<span style="color:gray; font-size: 11px; float: right;">Image credit: Midjourney.<br> Prompt: ‘lasso regression in style of engraving'
</span>
</div>
---

# Regularization Methods

* Here we add $\mathrm{RSS}$ penalty to shrink model coefficients to zero during fitting
	* This results in fewer predictions and lower variability of coefficients <br>
	$\mathrm{RSS}(\lambda, \beta, X) := \sum(y_i - \hat{y}_i)^2 + \lambda \lVert\beta\rVert_k$, $~~\lambda \geq 0$
		* where $\lambda$ is a **tuning parameter**, $\lambda \lVert\beta\rVert_k$ is a **shrinkage penalty**, $\beta := \beta_{1:p}$

<v-clicks depth="2">

* **Ridge**: $k = 2$, i.e. $\lVert\beta\rVert_2^2 = \beta^{\prime}\beta = \sum \beta_i^2$
	* a.k.a. a 2-norm, Euclidean distance (from zero), $L_2$, $L^2$, $L2$, $\ell_2$
		* It is the length of $\beta$ vector
	* $\beta_i$ gradually approach zero with larger $\lambda$
* **Lasso**: $k = 1$, i.e. $\lVert\beta\rVert_1 = \bm{1}^{\prime} \beta = \sum\lvert\beta_i\rvert$
	* a.k.a. a 1-norm, Manhattan (taxi-cab) distance, $L_1$, $L^1$, $L1$, $\ell_1$
	* $\beta_i$ gradually approach and then **snap to zero** with larger $\lambda$
</v-clicks>

---

# Regularization Methods

* $\mathrm{RSS}(\lambda, \beta, X) := \sum(y_i - \hat{y}_i)^2 + \lambda \lVert\beta\rVert_k$, $~~\lambda \geq 0$
* Both for **Ridge** and **Lasso**:
	* As $\lambda \to \infty$, optimizer focuses more on minimizing the penalty term
		* It's a **hyperparameter** we need to choose (by trying different values)
	* Note: $\beta_0$ is excluded from penalty b/c it does not relate predictors to response

---

# Regularization Methods: Example on Credit Data

* Coefficients for important features drop to zero slower
<div class="grid grid-cols-[7fr,8fr] gap-1">
<div>

* Important features are: <br> ``income``, ``limit``, ``rating``, ``student``
* Unimportant features are in gray
* $\uparrow\lambda \Rightarrow \uparrow$ model bias, $\downarrow \mathbb{V} Y$, <br> $\downarrow$ model complexity
</div>
<div>
<figure>
  <img src="/ISLP_figure_6.4.svg" style="width: 400px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 10px; left: 230px;">Ridge. Image source:
    <a href="https://hastie.su.domains/ISLR2/ISLP_website.pdf#page=249">ISLP Fig. 6.4</a>
  </figcaption>
</figure>
</div>
</div>
<br>
<div class="grid grid-cols-[7fr,8fr] gap-1">
<div>

* $r(\lambda) := \frac{\lVert\beta_{\lambda}^R\rVert_2}{\lVert\beta\rVert_2} \in [0, 1]$ is a 1-standardized penalty
	* $\uparrow r(\lambda) \Rightarrow \downarrow$ penalty
	* $\hat{\beta}_{\lambda}^R$ is a vector of best estimated ridge coefficients for each $\lambda$; $\hat{\beta}$ - LS coeffs.
</div>
<div>
<figure>
  <img src="/ISLP_figure_6.6.svg" style="width: 400px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 10px; left: 230px;">Lasso. Image source:
    <a href="https://hastie.su.domains/ISLR2/ISLP_website.pdf#page=253">ISLP Fig. 6.6</a>
  </figcaption>
</figure>
</div>
</div>

---

# Regularization Methods: Bayesian Formulation
<v-clicks depth="2">

* Recall that a $\mathrm{RSS}$ loss in Ordinary Least Squares may be written as: <br>
$\mathcal{L}(\hat{Y}, Y) = (Y - \hat{Y})^T (Y - \hat{Y}) = - \mathrm{ln~p}_{\mathcal{N}} (Y | \hat{Y}, \hat{\sigma}^2 I) + \mathrm{Const.}$
* Which allows us to re-write the solution of $\mathrm{OLS}$ as **Maximum Likelihood Estimation** (MLE) problem: <br>
$W_{\mathrm{OLS}} = \argmax\limits_W \{\mathrm{ln~p}_{\mathcal{N}} (Y | X, W)\}$
* Let's use a Bayes theorem instead!
	* If we assume some initial distribution on weights $p(W)$, we can update it in the following way: $\mathrm{p}(W | X, Y) \propto \mathrm{p} (Y | X, W) \mathrm{p}(W)$
* This converts MLE problem into **Maximum a Posteriori** (MAP) estimation problem: <br>
$W_{\mathrm{MAP}} = \argmax\limits_W \{\mathrm{ln~p}_{\mathcal{N}} (Y | X, W) + \mathrm{ln~p}(W) \}$
</v-clicks>

---

# Regularization Methods: Bayesian Formulation

* $W_{\mathrm{MAP}} = \argmax\limits_W \{\mathrm{ln~p}_{\mathcal{N}} (Y | X, W) + \mathrm{ln~p}(W) \}$
	* If $\mathrm{p}(W)$ is a standard Normal distribution, we get **Ridge regression**
	* If $\mathrm{p}(W)$ is a standard Laplace distribution, we get **Lasso regression**
	* If we generalize this idea from using a single weight estimate to a distribution of weights and use standard Normal prior $\mathrm{p}(W)$, we get **Gaussian Process**

---

# Regularization Methods: Lagrangian Formulation

* In optimization theory, the following are equivalent for $\beta = \beta_{1:p}$, $k = 1, 2$: <br>
$\hat{\beta}_{0:p} = \argmin\limits_{\forall \beta} \{\mathrm{RSS}_{\beta_0, \beta} + \lambda \lVert\beta\rVert_k\} = \argmin\limits_{\forall \beta} \{\mathrm{RSS}_{\beta_0, \beta} ~|~ \lVert \beta \rVert_k \leq s\}$
	* where: $\mathrm{RSS}_{\beta_0, \beta}$ is **objective function**, $\lVert \beta \rVert_k \leq s$ is **constraint (budget)**
	* For any $\lambda$, we can find some $s$ that yields the same minimum
	* Large budget wields unconstrained OLS
* We can also express the best subset selection (BSS) as: <br>
$\hat{\beta}_{0:p} = \argmin\limits_{\forall \beta} \{ \mathrm{RSS}_{\beta_0, \beta} ~|~ \lVert\beta\rVert_{\textcolor{red}{0}} \leq s\}$
	* where $\lVert\beta\rVert_0 = \sum I \{\beta_i \neq 0 \}$, i.e. just a count of coefficients is constrained

---

# Regularization Methods: Lasso vs. Ridge

* We can use Lagrangian formulation to compare Lasso and Ridge models

<br>
<div>
<figure>
  <img src="/ridge_lasso_explained.png" style="width: 690px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: -50px; left: 650px;">Based on:
    <a href="https://hastie.su.domains/ISLR2/ISLP_website.pdf#page=255">ISLP Fig. 6.7</a>
  </figcaption>
</figure>
</div>

<!-- #### The constraint region increases with $\lambda$
<Arrow x1="550" y1="125" x2="553" y2="260" />
<Arrow x1="550" y1="125" x2="815" y2="263" /> -->
