# Regression Splines

* Piecewise polynomial on intervals between cutpoints (a.k.a. **knots**)
	* We prefer low-order $d$ polynomials
<br> $y | x := \sum\limits_{k=0}^{K} P_{dk}(x) \cdot I(c_i \leq X \leq c_{i+1})$
<br> $P_{dk}(x) := \beta_{0k} + \beta_{1k}x + \beta_{2k}x^2 +...+ \beta_{dk}x^d$
* E.g. For a single knot $c = 50$ and $d = 3$ we have:
<br> $y(x) = \begin{cases}
        \beta_{01} + \beta_{11}x + \beta_{21}x^2 + \beta_{31}x^3 + \epsilon, \mathrm{\textcolor{grey}{~if}~} x < 50 \\
        \beta_{02} + \beta_{12}x + \beta_{22}x^2 + \beta_{32}x^3 + \epsilon, \mathrm{\textcolor{grey}{~if}~} x \geq 50
      \end{cases}$
	* We fit a separate polynomials on each sub-interval
	* We need to estimate 8 coefficients, i.e. degrees of freedom (DF)
* $K$ knots yield $K+1$ polynomials, each with $d+1$ coefficients
  * Higher DF implies higher complexity, higher variance, lower bias and risk of overfitting

---

# Ex. Wage vs Age

<div class="grid grid-cols-[3fr,4fr] gap-2">
<div>
<br>

* We have a knot at $50$ years of age
  * Does it make sense?
  * Any change in wage pattern?
* Degrees of freedom (DF) = number of estimated coefficients
* Each constraint **frees** up 1 DF
* A **cubic spline** with $K$ knots has <br> $\mathrm{DF} = 4 + (4-3)\cdot K = 4 + K$
  * vs. order 3 regression spline: <br> $\mathrm{DF} = (3+1)(K+1) =$ <br> $= 4 + 4K$
</div>
<div>
  <figure>
    <img src="/ISLRv2_figure_7.3.png" style="width: 480px !important;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 1px; left: 310px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=304">ISLP Fig. 7.3</a>
    </figcaption>
  </figure>
</div>
</div>

---

# Spline Basis Representation

* We can use the previous basis representation for cubic spline as well
	* It allows us to apply least squares estimation theory
  * Another representation of a piecewise polynomial representation with some basis:
<br> $y | x := \beta_0 + \beta_1 b_1 (x) +...+ \beta_{K+3} b_{K+3}(x) + \epsilon$
  * For this representation, we have basis $[x, x^2, x^3]$ and **truncated (cubic) power basis**
<br> $\beta_i := h(x, \xi)_{+}^3 = (x - \xi)^3 \cdot I(x > \xi) = \begin{cases}
        (x - \xi)^3, \mathrm{\textcolor{grey}{~if}~} x > \xi \\
        0, \mathrm{\textcolor{grey}{~otherwise}~}
      \end{cases}$
    * $h(x, \xi)_{+}^3$ is positive part of monomial $x^3$ centered at knot $\xi$

<!-- <div>
  <figure>
    <img src="/The-truncated-cubic-power-functions-x-i-3-i-0-3.png" style="width: 250px !important;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: -60px; left: 270px;">Image source:<br>
      <a href="href">https://www.researchgate.net/figure/<br>The-truncated-cubic-power-functions-x-i-3-i-0-3_fig3_251922295</a>
    </figcaption>
  </figure>
</div> -->

<!-- <div>
  <figure>
    <img src="/ESL_figure_5.1d.png" style="width: 250px; position: relative; top: -120px;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: -110px; left: 35px;">Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=161">ESL Fig. 5.1</a>
    </figcaption>
  </figure>
</div> -->

---

# Cubic Truncated Power Functions

* The coefficient $\beta$ in $\beta \cdot h(x, \xi)$ controls direction and scaling of the cubic curve

<br>

<div>
  <figure>
    <img src="/4-Figure2-1.png" style="width: 700px;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 0px; left: 605px;">Image source: <a href="https://doi.org/10.1155/2017/7518035">https://doi.org/10.1155/2017/7518035</a>
    </figcaption>
  </figure>
</div>

---

# Natural Cubic Spline (NCS)

* Cubic splines are highly **unstable** at the outer range of predictors
* NCS adds a boundary constraints: **linearity** at the boundaries
  * Also, there is insignificant difference in the fitted function
* Often NCS are more stable, as compared to same-DF polynomial regression

<div class="grid grid-cols-[1fr,1fr]">
<div>
  <figure>
    <img src="/ISLRv2_figure_7.4.png" style="width: 380px; position: absolute;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 270px; left: 15px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=306">ISLP Fig. 7.4</a>
    </figcaption>
  </figure>
</div>

<div>
  <figure>
    <img src="/ISLRv2_figure_7.7.png" style="width: 380px; position: absolute;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 270px; left: 300px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=309">ISLP Fig. 7.7</a>
    </figcaption>
  </figure>
</div>
</div>

---

# How to Select the Knots Hyperparameter, $\{\xi_i\}_{1:K}$

* If possible, we place knots where we expect the distribution of $Y | X$ to change
* Place more knots where you need a **higher resolution** to the changes in $Y | X$
  * i.e. where $Y | X$ is volatile function
* Place knots at regular intervals
  * Often used in practice
* Use CV and hyperparameter grid search to choose $K$ and locations of $\xi_1,...,\xi_K$

<div>
  <figure>
    <img src="/ISLRv2_figure_7.6.png" style="width: 460px; position: absolute;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 150px; left: 500px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=308">ISLP Fig. 7.6</a>
    </figcaption>
  </figure>
</div>

---

# Smoothing Splines

* Here we consider constrained function $g(x)$ to minimize
<br> $\mathrm{PRSS} := \sum\limits_{i}^n (y_i - g(x_i))^2 + \lambda \int g''(t)^2 dt$, where $g''$ is the **roughness** of $g$
<br> It is equivalent to a natural cubic spline with knots at $x_1,...,x_n$

* Large $|g''(t)|$ is a result of highly variable $g$ around $t$ 
* $\int g''(t)^2 dt$ measures the total change of $g'(t)$ over its support
* Smooth $g$ implies nearly constant $g'$, which implies $\int g''(t)^2 dt$ to be a small value
* Larger $\lambda$ implies smoother $g$
  * $\lambda = 0$ implies $\mathrm{RSS} = 0$ and $g$ pass through every point $(x_i, y_i)$
  * $\lambda >> 1$ implies $g$ be perfectly smooth and $y | x$ be a line with smallest $\mathrm{RSS}$
  * $\lambda$ controls bias-variance trade-off


---

# Effective Degree of Freedom (DF)

* A natural cubic spline with $n$ knots at $x_1,...,x_n$ has $\mathrm{DF} = n + 4$
* But, smoothing splines has $\mathrm{DF}_\lambda \in [2, n]$ 
  * Higher $\lambda$ implier lower $\mathrm{DF}_\lambda$, i.e. lower flexibility of smoothing spline


<div class="grid grid-cols-[1fr,1fr]">
<div>
<br>

$\mathrm{DF}_\lambda = \mathrm{trace} S_\lambda = \sum\limits_i^N \{S_\lambda\}_{ii}$
  * where $S_\lambda$ is $n \times n$ matrix,
  <br> which solves $\hat{g}_\lambda = S_\lambda y$, where $\hat{g}_\lambda$ is an $n$-vector
    * More details in <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=173">ESL book, p.154</a>
</div>

<div>
  <figure>
    <img src="/ISLRv2_figure_7.8.png" style="width: 440px; position: absolute;">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; top: 300px; left: 320px;">Image source:
      <a href="https://hastie.su.domains/ISLP/ISLP_website.pdf#page=312">ISLP Fig. 7.8</a>
    </figcaption>
  </figure>
</div>
</div>