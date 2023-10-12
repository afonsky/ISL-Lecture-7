---
background: /midjourney_subset_selection_in_style_of_engraving.jpg
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

# Subset Selection

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
<span style="color:gray; font-size: 11px; float: right;">Image credit: Midjourney.<br> Prompt: ‘subset selection in style of engraving'
</span>
</div>
---

# Best Subset Selection (BSS)

* We fit a model for every possible combination of $p$ features and keep the "best"
	* "best" is in terms of
		* $\mathrm{RSS}$, $\mathrm{MSE}$, $R^2$, $R_{adj}^2$, etc. on the validation set
			* Train set metrics overfit
		* **Mallow's** $C_p$, $\mathrm{AIC}$, $\mathrm{BIC}$ — mathematical estimates of test MSE
	* $p$ features yield $2^p$ model fits
		* $2^{60} = 1~152~921~504~606~846~976$ fittings for just $60$ features

---

# Best Subset Selection (BSS)
<div class="bg-orange-100">

* ### Algorithm: BSS
	1. Start with a **null model** with no predictors, $\mathcal{M}_0:~\hat{y} = \bar{y}$
	2. For $k = 1:p$
		1. Fit $\binom{p}{k} = \frac{p!}{k!(p-k)!}$ models with $k$ predictors
		2. Keep the **train-best** model. Call it $\mathcal{M}_k$
	3. Return the **train-best** of $\mathcal{M}_0, ... \mathcal{M}_p$
</div>

* In step 2, we could also use some quick (theoretical) estimate of test error

---

# Best Subset Selection (BSS)

<div class="grid grid-cols-[5fr,8fr] gap-6">
<div>

* We convert qualitative variables to $0-1$ dummies
	* Ethnicity has 3 levels, yielding 2 dummies (one is dropped to avoid perfect collinearity)
</div>
<div>
  <figure>
    <img src="/card_balance_table.png" style="width: 500px; position: relative">
  </figure>
</div>
</div>

<div class="grid grid-cols-[5fr,8fr] gap-6">
<div>

* Ex. For $k = 10$ combinations of features out of $p = 10$ total features, we need to fit <br> $\binom{10}{1} = \frac{10!}{1!(10-1)!}$ models
* Fewer features $\Rightarrow$ faster fit
</div>
<div>
<figure>
  <img src="/ISLP_figure_6.1.svg" style="width: 400px !important;">
  <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 10px; left: 260px;">Image source:
    <a href="https://hastie.su.domains/ISLR2/ISLP_website.pdf#page=240">ISLP Fig. 6.1</a>
  </figcaption>
</figure>
</div>
</div>

---

# Stepwise Selection

* A "**greedy**" search for best model, i.e. at every step we **optimize locally**, not globally
	* Much faster than BSS for larger $p$. Non-exhaustive

<div class="bg-orange-100">

* ### Algorithm: Forward Stepwise Selection
	1. Start with a **null model** with no predictors, $\mathcal{M}_0:~\hat{y} = \bar{y}$
	2. For $k = 0:\mathrm{min}(p-1, n)$
		1. Extend $\mathcal{M}_k$ by one feature from the remaining $p-k$ features <br> by trying each one separately
		2. Keep the **train-best** model. Call it $\mathcal{M}_{k+1}$
	3. Return the **train-best** of $\mathcal{M}_0, ... \mathcal{M}_p$
</div>

* We fit $1 + \frac{p(p+1)}{2}$ models. $60$ features $\Rightarrow 466$ models
	*  In step 2, we could also use some quick-estimate of test error

---

# Stepwise Selection

* A "**greedy**" search for best model, i.e. at every step we **optimize locally**, not globally
<v-clicks depth="2">

* Modifications:
	* **Backward Stepwise Selection**
		* Start with all features and drops them one by one
			* This selection method fails for least squares estimation, when $p > n$
	* **Mixed Stepwise Selection**
		* Add "best" and drop "worst" features in each cycle
</v-clicks>

---

# Stepwise Selection: Objective

* Optimally, we care to **minimize the test error** not train error!
<v-clicks depth="4">

* Two ways to estimate test error:
	* **Directly**: Use $k$-fold cross validation (CV) approach
		* Fewer assumptions about the underlying model and population
	* **Inderectly**: Mathematical adjustment to $\mathrm{RSS}$ based on $\hat{\sigma}^2 := \hat{\sigma}_{\epsilon}^2$ and $d$ predictors
		* **Mallow's** $C_p := \frac{1}{n}(\mathrm{RSS} + \color{grey}\underbrace{\color{#006}2d\hat{\sigma}^2}_{\mathrm{RSS~penalty}}\color{#006}) = \mathrm{MSE} + \frac{2d\hat{\sigma}^2}{n}$
		* **Akaike Information Criterion**, $\mathrm{AIC} := \frac{1}{n\hat{\sigma}^2} (\mathrm{RSS} + 2d\hat{\sigma}^2) = \frac{1}{\hat{\sigma}^2} C_p$
			* Alt: $C_p^{\prime} := \frac{\mathrm{RSS}{\hat{\sigma}^2}} + 2d - n$, then $C_p = \frac{1}{n} \hat{\sigma}^2 (C_p^{\prime} + n)$
		* **Bayesian Information Criterion**, $\mathrm{BIC} := \frac{1}{n\hat{\sigma}^2} (\mathrm{RSS} + (\log n)\hat{\sigma}^2)$

</v-clicks>