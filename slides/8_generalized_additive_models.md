---
layout: chapter-title

# the image source
image: /midjourney_GAM.jpg

# a custom class name to the content
class: my-cool-content-on-the-right
---

## Generalized Additive Models

<span style="color:DimGray; font-size: 11px; position:absolute; right:20px; bottom:20px;">Image credit: Midjourney<br> prompt: ‘generalized additive models in style of engraving'
</span>

---

# Generalized Additive Models

* Generalized Additive Models (GAMs) allows us to fit a non-linear functions $f_j$ to each $X_j$, while maintaining **additivity**
* The non-linear fits can potentially make more accurate predictions for the response $Y$
* B/c the model is additive, we can examine the effect of each $X_j$ on $Y$ individually while holding all of the other variables fixed
* The smoothness of the function $f_j$ for the variable $X_j$ can be summarized via degrees of freedom
* Limitation: **the model is restricted to be additive**
  * Feature engineering is needed:
    * One can include additional predictors of the form $X_j \times X_k$
      * Such terms can be fit using $2d$ smoothers or $2d$ splines

---

# GAMs for Regression Problems

* Multiple regression model: $y_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + ... + \beta_p x_{ip} + \epsilon_i$
* Replacing each **linear component** $\beta_j x_{ij}$ with a **smooth** non-linear function $f_j (x_{ij})$: <br>
$y_i = \beta_0 + \sum\limits_{j=1}^p f_j (x_{ij}) + \epsilon_i = \beta_0 + f_1 (x_{i1}) + f_2 (x_{i2}) + ... + f_p (x_{ip}) + \epsilon_i$
  * It is called an **additive** model because we calculate a separate $f_j$ for each $X_j$, and then add together all of their
contributions
* Ex. Wage
  * Let $f_j$ be natural splines. Then: <br> $\mathrm{wage} = \beta_0 + f_1({\mathrm{year}}) + f_1({\mathrm{age}}) + f_1({\mathrm{education}}) + \epsilon$
  * $\mathrm{year}$ and $\mathrm{age}$ are quantitative variables
  * $\mathrm{education}$ is qualitative variable with 5 levels: `<HS`, `HS`, `<Coll`, `Coll`, `>Coll`

---

# GAMs for Regression Problems

<div class="grid grid-cols-[3fr,4fr] gap-2">
<div>
<br>

* $f_1$ - natural spline, $\mathrm{DF} = 4$
* $f_2$ - natural spline, $\mathrm{DF} = 5$
* $f_3$ - step function
</div>
<div>
  <figure>
    <img src="/ISLP_figure_7.11.png" style="width: 450px !important;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 5px; left: 310px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=314">ISLP Fig. 7.11</a>
    </figcaption>
  </figure>
</div>
</div>

<div class="grid grid-cols-[3fr,4fr] gap-2">
<div>
<br>

* $f_1$ - smoothing spline, $\mathrm{DF} = 4$
* $f_2$ - smoothing spline, $\mathrm{DF} = 5$
* $f_3$ - step function
</div>
<div>
<br>
  <figure>
    <img src="/ISLP_figure_7.12.png" style="width: 450px !important;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 5px; left: 310px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=315">ISLP Fig. 7.12</a>
    </figcaption>
  </figure>
</div>
</div>

---

# GAMs for Classification Problems

* Logistic regression model: $\log(\frac{p(X)}{1 - p(X)}) = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + ... + \beta_p X_p$
* Logistic regression GAM: <br>
$\log(\frac{p(X)}{1 - p(X)}) = \beta_0 + \beta_1 f_1(X_1) + \beta_2 f_1(X_2) + ... + \beta_p f_p(X_p)$
* Ex. Wage
  * Let $f_2$ be smoothing spline, and $f_3$ be step funtion. <br> Then Logistic regression GAM is: <br> $\log(\frac{p(X)}{1 - p(X)}) = \beta_0 + \beta_1 \times{\mathrm{year}} + f_2({\mathrm{age}}) + f_3({\mathrm{education}})$
    * Where $p(X) = \mathrm{Pr}(\mathrm{wage} > 250 | \mathrm{year}, \mathrm{age}, \mathrm{education})$

---


# GAMs for Classification Problems

<div class="grid grid-cols-[3fr,4fr] gap-2">
<div>
<br>

* $f_2$ - smoothing spline, $\mathrm{DF} = 5$
* $f_3$ - step function
* $\mathrm{education}$ levels: <br>`<HS`, `HS`, `<Coll`, `Coll`, `>Coll`
</div>
<div>
  <figure>
    <img src="/ISLP_figure_7.13.png" style="width: 450px !important;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 5px; left: 310px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=317">ISLP Fig. 7.13</a>
    </figcaption>
  </figure>
</div>
</div>

<div class="grid grid-cols-[3fr,4fr] gap-2">
<div>
<br>

* $f_2$ - smoothing spline, $\mathrm{DF} = 5$
* $f_3$ - step function
* $\mathrm{education}$ levels: <br>`HS`, `<Coll`, `Coll`, `>Coll`
</div>
<div>
<br>
  <figure>
    <img src="/ISLP_figure_7.14.png" style="width: 450px !important;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 5px; left: 310px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=318">ISLP Fig. 7.14</a>
    </figcaption>
  </figure>
</div>
</div>