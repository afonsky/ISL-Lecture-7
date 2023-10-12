---
layout: image-left

# the image source
image: /midjourney_polynomial_regression.jpg

# a custom class name to the content
class: my-cool-content-on-the-right
---


# Polynomial regression


<span style="color:gray; font-size: 11px; float: right;">Image credit: Midjourney<br> Prompt: ‘polynomial regression in style of engraving'
</span>

---

# Polynomial Regression

* Consider a simple linear regression (SLR) $~y|x = \beta_0 + \beta_1 x + \epsilon$
* We can add **monomials** of $x$ to derive a polynomial function of degree $d$:
<br>$y|x = \beta_0 + \beta_1 x + \beta_2 x^2 + ... + \beta_d x^d + \epsilon$, where these $\beta_i$ are different from SLR's

<br>
<div class="grid grid-cols-[3fr,4fr] gap-1">
<div>

* E.g. Wage vs. Age:
<br> $\mathbb{P}[\mathrm{Wage > 250 | Age}] = \frac{e^{y|x}}{1 + e^{y|x}}$
* Logistic regression with $d=4$ with the $2\times$ estimated standard error (SE) of $\hat{f}$ at every point
  * Larger $d$ will lead to overfitting
* $2\times \mathrm{SE}$ is approximately a $95\%$ CI
</div>
<div>
  <figure>
    <img src="/ISLRv2_figure_7.1.png" style="width: 550px !important;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 10px; left: 350px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=299">ISLP Fig. 7.1</a>
    </figcaption>
  </figure>
</div>
</div>

* $99\%$ confidence interval would be wider

---

# Polynomial Regression: Pros and Cons

* **Pros**:
  * It captures a non-linear (polynomial) relation between input and output
* **Cons**:
  * A single polynomial imposes a **constraint on global structure**
    * I.e. it can’t control its flexibility on an interval in the support
    * In some situations, this can be a desirable behavior for a model
