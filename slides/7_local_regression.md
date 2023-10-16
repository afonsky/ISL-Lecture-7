---
layout: chapter-title

# the image source
image: /midjourney_local_regression.jpg

# a custom class name to the content
class: my-cool-content-on-the-right
---

## Local Regression

<span style="color:DimGray; font-size: 11px; position:absolute; right:20px; bottom:20px;">Image credit: Midjourney<br> prompt: ‘local regression in style of engraving'
</span>

---

# Local Regression

<div class="grid grid-cols-[4fr,4fr] gap-2">
<div>
<br>

* Fitting flexible functions at a target point $x_0$ using only **nearby** training observations
* Measure of the proximity is **span** $s = k/n$ - the proportion of points used to compute the local regression at $x_0$
</div>
<div>
  <figure>
    <img src="/ISLP_figure_7.9.png" style="width: 420px !important;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 5px; left: 240px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=312">ISLP Fig. 7.9</a>
    </figcaption>
  </figure>
</div>
</div>

<div class="grid grid-cols-[4fr,3fr] gap-2">
<div>
<br>

* Span controls the flexibility of the non-linear fit:
  * Smaller $s$: more **local** and wiggly fit
  * Larger $s$: towards global fit 
</div>
<div>
<br>
  <figure>
    <img src="/ISLP_figure_7.10.png" style="width: 300px !important;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: -10px; left: 240px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=313">ISLP Fig. 7.10</a>
    </figcaption>
  </figure>
</div>
</div>

---

# Local Regression Algorithm

<div class="bg-orange-100">

### Algorithm: Local Regression at $X = x_0$
  1. Gather the fraction $s = k/n$ of training points whose $x_i$ are closest
to $x_0$
  2. Assign a weight $K_{i0} = K(x_i , x_0)$ to each point in this neighborhood, so that the point furthest from $x_0$ has weight zero, and the closest has the highest weight. All but these $k$ nearest neighbors get weight zero
  3. Fit a weighted least squares regression of the $y_i$ on the $x_i$ using the aforementioned weights, by finding $\hat\beta_0$ and $\hat\beta_1$ that minimize <br> $\sum\limits_{i=1}^n K_{i0}(y_i - \beta_0 - \beta_1 x_i)^2$
  4. The fitted value at $x_0$ is given by $\hat{f}(x_0) = \hat\beta_0 + \hat\beta_1 x_0$
</div>

---

# Generalization of Local Regression

* While using multiple features $X_1, X_2, ..., X_p$:
  * One can apply *varying coefficient models*
    * Fitting a multiple linear regression model that is **global** in some variables, <br> but **local** in another
      * Catching seasonal effects in time series
  * Fit models that are **local** in a pair of variables $X_1, X_2$ rather than one variable
    * Use $2d$-neighborhoods and fit bivariate linear regression models
  * Fit to $p$-dimensional neighborhoods
    * $p \lesssim 4$ because there will generally be very few training observations close to $x_0$