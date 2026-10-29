# Chapter 2 — Probability Theory and Random Variables

## Source, scope, and notation

**Primary source:** A. H. Jazwinski, *Stochastic Processes and Filtering Theory*, Chapter 2, printed pp. 8–46, in `docs/Stochastic Process and Filter Theory/Stochastic Processes and Filtering Theory-21-59.pdf`. The file contains the complete chapter, including its references. Page citations below refer to printed book pages, not PDF page indices.

These notes follow the chapter's six sections while developing one mathematical narrative. Statements identified by textbook theorem, example, or equation numbers come from the chapter. Passages marked **Supplementary** provide additional interpretation, derivations, or connections to estimation and filtering; they are not additional claims attributed to Chapter 2.

The book writes random variables in lowercase roman type, realizations in lowercase italic type, probability as $\Pr$, and expectation as $\mathcal E$. Here, uppercase $X,Y,Z$ denote real random variables or column random vectors, lowercase $x,y,z$ denote realizations, $P(A)$ denotes the probability of an event, and $\mathbb E$ denotes expectation. This typographic modernization preserves the source's meanings. We retain its covariance notation:

$$
m_X=\mathbb E[X],\qquad
P_X=\operatorname{Cov}(X),\qquad
P_{XY}=\mathbb E[(X-m_X)(Y-m_Y)^T].
$$

For $X\in\mathbb R^n$ and $Y\in\mathbb R^m$, $P_X\in\mathbb R^{n\times n}$, $P_Y\in\mathbb R^{m\times m}$, and $P_{XY}\in\mathbb R^{n\times m}$, with $P_{YX}=P_{XY}^T$. Covariance formulas require finite second moments. Matrices and affine offsets are deterministic unless stated otherwise. All conditional identities are understood almost surely unless a particular version at a specified observation is given.

**Source qualifications.** The chapter deliberately uses an elementary, largely density-based treatment, assumes appropriate smoothness, and generally works with Riemann integrals. These notes use standard measurable-variable language to state applicability precisely without reproducing a measure-theoretic development. On p. 16, the source informally equates absolute continuity with having only countably many points of nondifferentiability. That characterization is not a general definition of absolute continuity; the density representation stated below is the condition used here. The source's full-dimensional Gaussian densities require positive definite covariance matrices; singular Gaussian distributions are handled separately in Section 6.6.

## A. Chapter overview — The big picture

Filtering estimates an unobserved state from observations contaminated by uncertainty. Before describing a time-evolving state, one needs a language for possible outcomes, numerical quantities associated with them, statistical dependence, and changes in knowledge after observation. Chapter 2 supplies that language through the progression

$$
\text{Probability space}
\longrightarrow \text{Random variables}
\longrightarrow \text{Joint distributions}
\longrightarrow \text{Conditional probability}
\longrightarrow \text{Gaussian conditioning}
\longrightarrow \text{Filtering theory}.
$$

A distribution represents uncertainty. A joint distribution records how uncertain quantities are related. Conditioning converts those relationships into information about a hidden quantity. Gaussian structure then makes certain otherwise infinite-dimensional distribution calculations reducible to calculations involving means and covariance matrices. The final step toward recursive filtering is a forward-looking application, developed explicitly in Section I.

## B. Section 1 — Introduction: from a physical problem to a probability model

*Source: Section 2.1, pp. 8–9.*

The author separates stochastic problem solving into three steps:

1. **Assign initial probabilities.** Specify a probabilistic model for selected events.
2. **Derive mathematical consequences.** Apply the rules of probability to compute probabilities of other events.
3. **Interpret physical predictions.** Use the computed probabilities to predict and assess outcomes in the original experiment.

For example, assigning probability $1/6$ to each face of a fair die gives probability $1/2$ to an even result. Predicting roughly 500 even results in 1,000 trials is a subsequent interpretation of the model; it is not a promise of exactly 500 results.

In the author's pedagogical distinction, probability theory concerns the deductive second step, while assigning probabilities and interpreting predictions belong to statistics. This is the chapter's framing rather than a comprehensive modern definition of either discipline. Initial probabilities may be motivated by relative frequencies, equally likely alternatives, or degrees of belief.

**Mathematically correct inference does not guarantee a physically correct probabilistic model.** A calculation can be internally consistent while resting on an incorrect assumption about a die, a sensor, or a disturbance.

**Supplementary filtering interpretation.** A filter may correctly compute the posterior under its assumed noise distribution and still provide inaccurate or poorly calibrated estimates when the actual sensor has bias, heavy tails, or dependence omitted from the model. Model specification and validation therefore remain distinct from the algebra of inference.

## C. Section 2 — Probability axioms: the common mathematical foundation

*Source: Section 2.2, pp. 10–13.*

### 2.1 Probability space and measurable events

A probability space is the triple

$$
(\Omega,\mathcal B,P).
$$

Here $\Omega$ is the sample space, $\omega\in\Omega$ is an elementary outcome, and $\mathcal B$ is the collection of events to which probabilities are assigned. It contains $\Omega$ and is closed under complements and countable unions, hence also countable intersections. In modern terminology, $\mathcal B$ is a $\sigma$-algebra; the chapter calls such an event class a **Borel field**, and calls the triple an **experiment**.

An event is a measurable subset $A\in\mathcal B$. One should not assume that every subset of an arbitrary sample space is measurable. The triple matters: a physical experiment alone does not uniquely determine its mathematical sample space or event class. Example 2.1 illustrates how different event classes can describe different levels of information about the same die experiment.

The probability function satisfies nonnegativity, normalization, and countable additivity:

$$
P(A)\geq 0,\qquad P(\Omega)=1,
$$

$$
P\!\left(\bigcup_{j=1}^{\infty}A_j\right)
=\sum_{j=1}^{\infty}P(A_j)
\quad\text{for pairwise disjoint }A_j\in\mathcal B.
$$

Countable additivity includes finite additivity and makes probabilities consistent with countable limiting operations. The elementary consequences in Eqs. (2.1)–(2.4) are

$$
\begin{aligned}
P(\varnothing)&=0,\\
P(A^c)&=1-P(A),\\
P(A\cup B)&=P(A)+P(B)-P(A\cap B),\\
A\subseteq B&\implies P(A)\leq P(B).
\end{aligned}
$$

### 2.2 Almost surely versus surely

A property holds **almost surely**, abbreviated a.s., if the event on which it holds has probability one. It holds **surely** if it holds for every outcome in $\Omega$. A probability-zero event need not be empty, and a probability-one event need not equal $\Omega$.

For a supplementary illustration, under the uniform law on $[0,1]$, every singleton has probability zero, yet some point in $[0,1]$ is realized. Thus a statement may fail at a nonempty null set and still hold almost surely. Random-variable identities and conditional expectations often identify quantities only up to such null sets. This distinction becomes essential when conditioning on continuously distributed observations.

## D. Section 3 — Random variables: from outcomes to distributions

*Source: Section 2.3, pp. 13–24.*

### 3.1 Random variables and distribution functions

A real random variable is a finite real-valued measurable function $X:\Omega\to\mathbb R$: for every real $x$,

$$
\{\omega:X(\omega)\leq x\}\in\mathcal B.
$$

This is the content of Eq. (2.5). Measurability ensures that numerical statements about $X$ correspond to events with defined probabilities. Its cumulative distribution function, or CDF, is

$$
F_X(x)=P(X\leq x).
$$

The CDF is nondecreasing and right-continuous, tends to zero as $x\to-\infty$, and tends to one as $x\to\infty$. It determines the distribution. In particular, Eqs. (2.10)–(2.11) give

$$
P(a<X\leq b)=F_X(b)-F_X(a),\qquad
P(X=x)=F_X(x)-F_X(x^-),
$$

where $F_X(x^-)$ is the left limit. Jumps encode probability masses.

For a **discrete** variable, a probability mass function, or PMF, assigns

$$
q_X(x)=P(X=x),\qquad
\sum_{x\in\mathcal S_X}q_X(x)=1,
$$

where $\mathcal S_X$ is a countable support. The chapter denotes this mass function by $m_x$; $q_X$ avoids confusion with the mean $m_X$.

For an **absolutely continuous** variable, a probability density function, or PDF, satisfies

$$
F_X(x)=\int_{-\infty}^{x}p_X(t)\,dt,\qquad
p_X(t)\geq0,\qquad
\int_{\mathbb R}p_X(t)\,dt=1.
$$

Then $p_X=F_X'$ almost everywhere and

$$
P(a<X\leq b)=\int_a^b p_X(t)\,dt.
$$

A density is probability per unit length, not probability at a point; $P(X=x)=0$ for every fixed $x$ in this case. For small $\Delta x$, $p_X(x)\Delta x$ approximates the probability of a short interval when the density is sufficiently regular.

**Scope qualification.** The chapter calls variables possessing such a density “continuous,” later mentions mixed laws, and then focuses on smooth densities. A continuous CDF alone does not guarantee a PDF; these notes do not treat discrete and absolutely continuous laws as an exhaustive classification. Likewise, the chapter's Dirac-delta representation of a constant, Eq. (2.26), is formal: a constant has a point-mass distribution, not an ordinary Lebesgue density.

### 3.2 Expectation and functions of a random variable

For a scalar variable, a finite expectation requires $\mathbb E[|X|]<\infty$. With a density or a PMF,

$$
\mathbb E[X]=\int_{\mathbb R}x\,p_X(x)\,dx
\quad\text{or}\quad
\mathbb E[X]=\sum_{x\in\mathcal S_X}x\,q_X(x).
$$

For measurable $g$, if $\mathbb E[|g(X)|]<\infty$, Eq. (2.29) gives

$$
\mathbb E[g(X)]=\int_{\mathbb R}g(x)p_X(x)\,dx,
$$

with the analogous sum for a discrete variable. One can average $g(X)$ using the law of $X$ without first deriving the law of $g(X)$. Expectation is linear whenever the terms are integrable:

$$
\mathbb E[aX+bY+c]
=a\mathbb E[X]+b\mathbb E[Y]+c.
$$

Independence is not needed for linearity.

### 3.3 Moments describe selected aspects of uncertainty

The $r$th raw moment and central moment, Eqs. (2.32)–(2.33), are

$$
\mu_r'=\mathbb E[X^r],\qquad
\mu_r=\mathbb E[(X-m_X)^r],\qquad m_X=\mathbb E[X],
$$

when the required absolute moments are finite. Raw moments refer to the origin; central moments refer to the distribution's own mean. In particular,

$$
\operatorname{Var}(X)=\sigma_X^2
=\mathbb E[(X-m_X)^2]
=\mathbb E[X^2]-m_X^2,\qquad
\sigma_X=\sqrt{\operatorname{Var}(X)}.
$$

The mean records location. Variance records average squared dispersion around that location; standard deviation expresses dispersion in the units of $X$. Example 2.5 establishes the useful limiting case

$$
\mathbb E[(X-c)^2]=0\implies X=c\quad\text{a.s.}
$$

**Supplementary interpretation of higher moments.** The third central moment detects signed asymmetry; when $\sigma_X>0$, $\mu_3/\sigma_X^3$ is skewness. The fourth central moment weights large deviations strongly; $\mu_4/\sigma_X^4$ is kurtosis when finite. Neither is a complete description of distributional shape.

Mean and variance generally do not determine a distribution. For a supplementary example, a variable with probabilities $1/2$ at each of $-1$ and $1$, and a standard Gaussian variable, both have mean zero and variance one. Their distributions are different, and their fourth moments are respectively one and three. Mean and covariance determine a Gaussian law **because the Gaussian family has that special structure**.

### 3.4 Characteristic functions: distributions in Fourier form

The characteristic function, Eq. (2.36), is

$$
\varphi_X(u)=\mathbb E[e^{iuX}],\qquad u\in\mathbb R,\quad i^2=-1.
$$

It always exists because $|e^{iuX}|=1$, so $|\varphi_X(u)|\leq1$. Unlike moments, it does not require tail integrability. It determines the distribution, including distributions without a density.

When a PDF exists,

$$
\varphi_X(u)=\int_{\mathbb R}e^{iux}p_X(x)\,dx.
$$

This is the Fourier transform with the chapter's positive-exponent convention. Under the absolute-integrability assumption stated for Eq. (2.39), inversion recovers a density version:

$$
p_X(x)=\frac{1}{2\pi}\int_{\mathbb R}e^{-iux}\varphi_X(u)\,du.
$$

For moment extraction, if $\mathbb E[|X|^r]<\infty$, differentiation under the expectation gives

$$
\varphi_X^{(r)}(0)=i^r\mathbb E[X^r],\qquad
\mathbb E[X^r]=i^{-r}\varphi_X^{(r)}(0).
$$

This finite-order integrability condition is a supplementary refinement of the source's analytic-function qualification in Eq. (2.37). The existence of a characteristic function does not itself imply the existence of moments of all orders.

If $X$ and $Y$ are independent, Example 2.15 and Eq. (2.90) show that

$$
\begin{aligned}
\varphi_{X+Y}(u)
&=\mathbb E[e^{iuX}e^{iuY}]\\
&=\mathbb E[e^{iuX}]\mathbb E[e^{iuY}]
=\varphi_X(u)\varphi_Y(u).
\end{aligned}
$$

Thus sums become products in Fourier form. This is especially useful when combining independent Gaussian state uncertainty and measurement or process noise.

### 3.5 Gaussian random variables

For $\sigma^2>0$, the scalar Gaussian law, Eq. (2.40), is

$$
X\sim\mathcal N(m,\sigma^2),\qquad
p_X(x)=\frac{1}{\sqrt{2\pi\sigma^2}}
\exp\!\left[-\frac{(x-m)^2}{2\sigma^2}\right].
$$

Its parameters are exactly its mean and variance. Example 2.6 gives

$$
\varphi_X(u)=\exp\!\left(ium-\frac12\sigma^2u^2\right).
$$

The central moments in Eq. (2.48) are

$$
\mathbb E[(X-m)^{2r+1}]=0,\qquad
\mathbb E[(X-m)^{2r}]=(2r-1)!!\,\sigma^{2r},\quad r\geq1.
$$

Here $(2r-1)!!=1\cdot3\cdots(2r-1)$; odd central moments vanish by symmetry. In particular, the fourth central moment is $3\sigma^4$. The exponential-quadratic characteristic function makes Gaussian sums and affine transformations easy to identify. General nonlinear transformations need not preserve Gaussianity.

## E. Section 4 — Jointly distributed random variables: dependence and uncertainty propagation

*Source: Section 2.4, pp. 24–36.*

### 4.1 Joint laws and marginalization

Random variables are jointly distributed when defined on the same probability space. For two scalar variables,

$$
F_{X,Y}(x,y)=P(X\leq x,Y\leq y).
$$

When a joint density exists, integrating out an unneeded variable gives its companion's marginal density, as in Eq. (2.53):

$$
p_X(x)=\int p_{X,Y}(x,y)\,dy,\qquad
p_Y(y)=\int p_{X,Y}(x,y)\,dx.
$$

The joint law contains information about dependence that the two marginals alone do not supply. Marginalization discards a variable; conditioning instead retains its observed value as information. These are distinct operations.

### 4.2 Moment existence and covariance

For square-integrable scalar variables, the chapter's Schwarz inequality, Eq. (2.61), implies the familiar Cauchy–Schwarz bound

$$
|\mathbb E[XY]|^2\leq\mathbb E[X^2]\mathbb E[Y^2].
$$

It guarantees that the cross moment is finite. Theorems 2.1–2.4, pp. 27–28, supply the relevant closure properties:

- **Theorem 2.1:** $\mathbb E[|X|^2]<\infty$ implies $\mathbb E[|X|]<\infty$.
- **Theorem 2.2:** for integer orders $1\leq r\leq s$, $\mathbb E[|X|^s]<\infty$ implies $\mathbb E[|X|^r]<\infty$.
- **Theorem 2.3:** finite second moments of $X$ and $Y$ imply finite $\mathbb E[|X+Y|^2]$ and $\mathbb E[|XY|]$.
- **Theorem 2.4:** for a positive integer $r$, finite $\mathbb E[|X|^r]$ and $\mathbb E[|Y|^r]$ imply finite $\mathbb E[|X+Y|^r]$.

These results explain why finite-second-moment variables form a suitable setting for covariance calculations and quadratic estimation errors.

The scalar covariance and correlation coefficient, Eqs. (2.57)–(2.59), are

$$
\operatorname{Cov}(X,Y)
=\mathbb E[(X-m_X)(Y-m_Y)]
=\mathbb E[XY]-m_Xm_Y,
$$

$$
\rho_{XY}=\frac{\operatorname{Cov}(X,Y)}{\sigma_X\sigma_Y},
\qquad -1\leq\rho_{XY}\leq1.
$$

The coefficient requires finite, strictly positive variances. Covariance measures signed linear association; its magnitude depends on units, whereas correlation is normalized. Example 2.13 shows that $|\rho_{XY}|=1$ corresponds to an affine relation between the nondegenerate variables almost surely.

### 4.3 Independence, uncorrelatedness, and orthogonality

Independence is a property of the complete joint law. For all Borel sets $A,B$,

$$
P(X\in A,Y\in B)=P(X\in A)P(Y\in B).
$$

For variables with a joint density, this is equivalent to factorization $p_{X,Y}(x,y)=p_X(x)p_Y(y)$ almost everywhere. It implies factorization of expectations of integrable products of separate functions, as in Eq. (2.66).

For finite-second-moment variables, the three concepts compare as follows:

| Concept          | What it asserts                 | Scope                                          |
| ---------------- | ------------------------------- | ---------------------------------------------- |
| Independence     | Joint probabilities factor      | Full distributional relationship               |
| Uncorrelatedness | Centered cross moment is zero   | A second-order relationship                    |
| Orthogonality    | Uncentered cross moment is zero | A second-order relationship depending on means |

The defining second-order equations are

$$
\operatorname{Cov}(X,Y)=0\quad\text{(uncorrelated)},\qquad
\mathbb E[XY]=0\quad\text{(orthogonal)}.
$$

**Theorem 2.5, p. 29:** independence implies uncorrelatedness when the required second moments exist. The converse is false in general. Uncorrelated variables have orthogonal centered versions $X-m_X$ and $Y-m_Y$; they themselves are orthogonal if their mean product is zero.

**Textbook Example 2.10.** For $U$ uniform on $[0,1]$, let $X=\sin(2\pi U)$ and $Y=\cos(2\pi U)$. Then $\mathbb E[X]=\mathbb E[Y]=\mathbb E[XY]=0$, but $X^2+Y^2=1$. They are uncorrelated and orthogonal while remaining dependent. Their values lie on a circle, so the pair has no ordinary two-dimensional joint density; individual densities do not imply a joint density.

For scalar variables with finite variances,

$$
\operatorname{Var}(X+Y)
=\operatorname{Var}(X)+\operatorname{Var}(Y)
+2\operatorname{Cov}(X,Y).
$$

Thus adding variances needs zero covariance, not necessarily independence; this is the point of Example 2.11.

### 4.4 Random vectors and covariance geometry

For a column vector $X=(X_1,\ldots,X_n)^T$ with finite second moments, Eq. (2.79) defines

$$
P_X=\mathbb E[(X-m_X)(X-m_X)^T],\qquad
(P_X)_{ij}=\operatorname{Cov}(X_i,X_j).
$$

Diagonal entries are component variances. Off-diagonal entries describe pairwise linear dependence and the orientation of uncertainty relative to the coordinate axes. A diagonal covariance matrix means pairwise uncorrelated components; independence needs more information unless the vector is jointly Gaussian.

The matrix is symmetric because $(P_X)_{ij}=(P_X)_{ji}$. It is positive semidefinite because, for every deterministic $a\in\mathbb R^n$,

$$
a^TP_Xa
=\mathbb E[(a^T(X-m_X))^2]
=\operatorname{Var}(a^TX)\geq0.
$$

Thus $P_X\succeq0$: it assigns a nonnegative variance to every direction. If it is singular, some nonzero $a$ has $a^T(X-m_X)=0$ a.s., expressing an exact affine constraint. Positive definiteness excludes such a zero-variance direction.

### 4.5 Affine transformations propagate first and second moments

For any $X\in\mathbb R^n$ with finite second moments, let

$$
Y=AX+b,\qquad A\in\mathbb R^{q\times n},\quad b\in\mathbb R^q.
$$

Linearity of expectation and centering give

$$
m_Y=Am_X+b,\qquad Y-m_Y=A(X-m_X),
$$

and therefore

$$
\begin{aligned}
P_Y
&=\mathbb E[A(X-m_X)(X-m_X)^TA^T]\\
&=AP_XA^T.
\end{aligned}
$$

These formulas follow from the chapter's expectation and covariance definitions; **they do not require Gaussianity**. A translation shifts location without changing covariance, while a linear map rotates, stretches, projects, or collapses the uncertainty. Gaussianity is the extra condition needed to identify the entire transformed law from these parameters, as in Theorem 2.11.

**Supplementary filtering connection.** If a state prediction is $X_{k+1}=F_kX_k+b_k+W_k$, and the process noise is uncorrelated with the current state, then

$$
m_{k+1}=F_km_k+b_k+m_{W_k},\qquad
P_{k+1}=F_kP_kF_k^T+Q_k,\quad Q_k=P_{W_k}.
$$

If the cross-covariance $C_k=\operatorname{Cov}(X_k,W_k)$ is nonzero, the covariance also contains $F_kC_k+C_k^TF_k^T$. Zero cross-covariance suffices for the displayed moment propagation; joint Gaussian assumptions are additionally needed for Gaussian distributional propagation.

### 4.6 Transforming a complete density

**Theorem 2.7, pp. 34–35, Eq. (2.85).** Suppose $Y=f(X)$ maps between spaces of the same dimension, $f$ is one-to-one on the domain considered, $f$ and $f^{-1}$ are continuously differentiable, and the inverse Jacobian determinant is nonzero. Then

$$
p_Y(y)=p_X(f^{-1}(y))
\left|\det Df^{-1}(y)\right|.
$$

The determinant accounts for the local change of volume, preserving probability. This density formula requires stronger conditions than the affine moment formulas: a rectangular or singular linear map need not have an inverse or an ordinary output density.

In Example 2.15, summing two variables is handled by augmenting the transformation to an invertible two-dimensional map and then marginalizing. For independent variables with densities, this yields the convolution

$$
p_{X+Y}(s)=\int p_X(s-t)p_Y(t)\,dt.
$$

This is the density counterpart of characteristic-function multiplication and anticipates integration over uncertain previous states in filtering.

## F. Section 5 — Conditional probabilities and expectations: learning from observations

*Source: Section 2.5, pp. 36–42.*

### 5.1 Conditioning changes the relevant probability law

For events $A,B$ with $P(B)>0$, Eq. (2.91) defines

$$
P(A\mid B)=\frac{P(A\cap B)}{P(B)}.
$$

The occurrence of $B$ restricts the possibilities, and normalization makes the restricted probabilities sum to one. In textbook Example 2.16, learning that a fair die result is even changes the probabilities of 2, 4, and 6 to $1/3$ each.

For continuously distributed $Y$, however, $P(Y=y)=0$, so the event-ratio formula cannot directly define conditioning on an exact value. The chapter motivates the conditional density through shrinking intervals around $y$, then defines, for a jointly distributed pair with a joint density and $p_Y(y)>0$,

$$
p_{X\mid Y}(x\mid y)=\frac{p_{X,Y}(x,y)}{p_Y(y)},\qquad
p_Y(y)=\int p_{X,Y}(x,y)\,dx.
$$

This is Eq. (2.100), valid for scalar variables and vectors. It is a density construction, not division by the zero probability of the singleton event. More generally, conditional laws are specified only for almost every conditioning value; arbitrary changes on a set of zero $Y$-probability do not alter the underlying conditional random object.

### 5.2 Bayes' theorem: prior, likelihood, posterior, evidence

Combining the two factorizations of the joint density gives Bayes' theorem, Eq. (2.102):

$$
p_{X\mid Y}(x\mid y)
=\frac{p_{Y\mid X}(y\mid x)p_X(x)}{p_Y(y)},
\qquad
p_Y(y)=\int p_{Y\mid X}(y\mid x)p_X(x)\,dx.
$$

The prior $p_X(x)$ represents uncertainty before the observation. The likelihood $p_{Y\mid X}(y\mid x)$ evaluates how compatible each candidate state is with the fixed observed value $y$. The posterior $p_{X\mid Y}(x\mid y)$ is the updated state density. The evidence $p_Y(y)$ normalizes the product and describes the observation's marginal density. As a function of $x$, a likelihood need not integrate to one.

Bayes' theorem is general within this density setting; it does not assume Gaussianity, linear measurements, or a point estimator. Discrete versions use probability masses and sums.

**Theorem 2.8, pp. 39–40:** conditional densities are nonnegative and normalized, and averaging them over the observation recovers the prior:

$$
\int p_{X\mid Y}(x\mid y)\,dx=1,\qquad
p_X(x)=\int p_{X\mid Y}(x\mid y)p_Y(y)\,dy.
$$

If $X$ and $Y$ are independent, $p_{X\mid Y}(x\mid y)=p_X(x)$ almost everywhere. An independent observation supplies no information about the hidden variable.

### 5.3 Conditional expectation is a function of the available information

Assume $\mathbb E[\|X\|]<\infty$. In the density setting, Eq. (2.103) gives the conditional mean at a specified observation:

$$
g(y)=\mathbb E[X\mid Y=y]
=\int x\,p_{X\mid Y}(x\mid y)\,dx.
$$

For vectors this integral is componentwise. Before the observation is known,

$$
\mathbb E[X\mid Y]=g(Y)
$$

is itself a random variable or vector, because it depends on the random observation. After observing $Y=y$, $g(y)$ is a numerical value. Keeping $g(Y)$ and $g(y)$ distinct prevents confusion between a random estimator and its realized estimate. Integrability ensures existence almost surely; no claim is needed at every exceptional conditioning value.

The conditional mean compresses the posterior into an average. Outside special families it does not determine the posterior distribution, and it may depend nonlinearly on $y$.

### 5.4 Theorem 2.9: averaging and using known quantities

*Source: p. 40, Eqs. (2.109)–(2.115).*

For integrable random variables or vectors and deterministic scalars $a,b$,

$$
\mathbb E[aX+bZ\mid Y]
=a\mathbb E[X\mid Y]+b\mathbb E[Z\mid Y].
$$

For an integrable measurable function $h(Y)$,

$$
\mathbb E[h(Y)\mid Y]=h(Y).
$$

Once $Y$ is given, its functions are known. Under the required integrability of the products, the same idea gives

$$
\mathbb E[h(Y)X\mid Y]=h(Y)\mathbb E[X\mid Y],
$$

and hence

$$
\mathbb E[h(Y)X]
=\mathbb E[h(Y)\mathbb E[X\mid Y]].
$$

For the conditional pull-out identity, it suffices that $X$ and $h(Y)X$ be integrable; bounded $h$ is an especially simple sufficient case. Product integrability should not be omitted merely because individual expectations exist.

The **law of total expectation**, Eq. (2.110), is

$$
\mathbb E[X]=\mathbb E[\mathbb E[X\mid Y]].
$$

In a density model, the concise derivation is

$$
\begin{aligned}
\mathbb E[g(Y)]
&=\int\!\left[\int x\,p_{X\mid Y}(x\mid y)\,dx\right]p_Y(y)\,dy\\
&=\iint x\,p_{X,Y}(x,y)\,dx\,dy
=\mathbb E[X].
\end{aligned}
$$

Integrability justifies exchanging the integrations. Intuitively, first average within each observation group, then average across groups. If $X$ and $Y$ are independent, the theorem also gives $\mathbb E[X\mid Y]=\mathbb E[X]$. The reverse implication is not generally valid: a constant conditional mean need not imply independence.

### 5.5 Conditional covariance and the law of total covariance

For $\mathbb E[\|X\|^2]<\infty$, Example 2.17 defines the conditional covariance matrix in Eq. (2.116):

$$
P_{X\mid Y}
=\mathbb E\!\left[
(X-\mathbb E[X\mid Y])(X-\mathbb E[X\mid Y])^T
\mid Y\right].
$$

It measures uncertainty remaining after observing $Y$. In general it is a random matrix: its realization $P_{X\mid y}$ can depend on the observed value. It is symmetric and positive semidefinite almost surely.

The law of total covariance, Eqs. (2.117)–(2.118), is

$$
P_X=\mathbb E[P_{X\mid Y}]+P_{\mathbb E[X\mid Y]}.
$$

The first term is average remaining uncertainty. The second is the covariance of the conditional mean across possible observations: the variation explained by the available information.

To see the decomposition, put $g(Y)=\mathbb E[X\mid Y]$ and write

$$
X-m_X=(X-g(Y))+(g(Y)-m_X).
$$

The conditional residual has zero mean given $Y$:

$$
\mathbb E[X-g(Y)\mid Y]=0.
$$

Consequently the cross terms in the outer-product expansion vanish after conditioning, and total expectation gives the covariance identity. In particular,

$$
\mathbb E[P_{X\mid Y}]\preceq P_X.
$$

This is an **average** reduction. For a general distribution, one cannot infer $P_{X\mid y}\preceq P_X$ at every observation; some observations can identify a more variable subpopulation. The observation-independent covariance of a jointly Gaussian conditional law is a stronger special result established in Section 6.

### 5.6 Supplementary derivation: conditional expectation minimizes mean squared error

This estimation result follows from the chapter's conditional-expectation identities, but Chapter 2 does not state it as an estimator-optimality theorem.

Let $X\in\mathbb R^n$ have finite second moment. Among all measurable estimators $a(Y)\in\mathbb R^n$ with finite squared error, define

$$
J(a)=\mathbb E[\|X-a(Y)\|^2].
$$

Set $g(Y)=\mathbb E[X\mid Y]$. Expanding $X-a(Y)=(X-g(Y))+(g(Y)-a(Y))$ gives

$$
\begin{aligned}
J(a)
={}&\mathbb E[\|X-g(Y)\|^2]
+\mathbb E[\|g(Y)-a(Y)\|^2]\\
&+2\mathbb E[(X-g(Y))^T(g(Y)-a(Y))].
\end{aligned}
$$

The cross term is zero because its second factor is a function of $Y$ and the residual has zero conditional mean. Therefore

$$
\boxed{
J(a)=J(g)+\mathbb E[\|g(Y)-a(Y)\|^2]\geq J(g).
}
$$

Thus the conditional mean is the minimum mean squared error estimator, unique up to almost-sure equality. Its minimum expected squared Euclidean error is

$$
J(g)=\operatorname{tr}\!\left(\mathbb E[P_{X\mid Y}]\right).
$$

Gaussianity is unnecessary for this optimality. The special Gaussian result is that the unrestricted optimal estimator becomes affine in the observation. A non-Gaussian conditional mean may be nonlinear; an estimator restricted to affine functions solves a different optimization problem. Also, a residual orthogonal to functions of $Y$ need not be independent of $Y$.

## G. Section 6 — Properties of Gaussian random variables

*Source: Section 2.6, pp. 42–46; supporting definitions in Section 2.4.*

### 6.1 Joint Gaussian structure and block covariance

For $Z\in\mathbb R^d$ with $P_Z\succ0$, Eq. (2.80) gives the multivariate Gaussian density

$$
p_Z(z)=\frac{1}{(2\pi)^{d/2}\det(P_Z)^{1/2}}
\exp\!\left[-\frac12(z-m_Z)^TP_Z^{-1}(z-m_Z)\right].
$$

Its characteristic function, Eq. (2.84), is

$$
\varphi_Z(u)=\exp\!\left(iu^Tm_Z-\frac12u^TP_Zu\right).
$$

The characteristic-function expression remains meaningful for $P_Z\succeq0$ even when no full-dimensional density exists. We use $\mathcal N(m_Z,P_Z)$ for this possibly degenerate Gaussian law as well.

Partition a jointly Gaussian vector into state and observation blocks:

$$
Z=\begin{bmatrix}X\\Y\end{bmatrix},\qquad
m_Z=\begin{bmatrix}m_X\\m_Y\end{bmatrix},\qquad
P_Z=\begin{bmatrix}
P_X&P_{XY}\\
P_{YX}&P_Y
\end{bmatrix}.
$$

This is the block structure of Eqs. (2.131)–(2.132). The four theorems below concern this joint structure, not simply the separate Gaussianity of $X$ and $Y$.

**Supplementary counterexample: Gaussian marginals are insufficient.** Let $G\sim\mathcal N(0,1)$, let $S$ be independent of $G$ with probabilities $1/2$ at $-1$ and $1$, and set $X=G$, $Y=SG$. Each marginal is standard Gaussian and $\operatorname{Cov}(X,Y)=0$, but $|X|=|Y|$ a.s., so they are dependent. The pair is not jointly Gaussian. Its covariance matrix is the identity, which also shows that Gaussian marginals plus a positive definite covariance matrix still do not establish joint Gaussianity.

### 6.2 Theorem 2.10 — Uncorrelated jointly Gaussian vectors are independent

*Source: pp. 43–44; scalar-component precursor: Theorem 2.6, p. 33.*

**Statement.** If $Z=(X^T,Y^T)^T$ is jointly Gaussian and $P_{XY}=0$, then $X$ and $Y$ are independent; their marginals are Gaussian with the corresponding mean and covariance blocks. Conversely, independence implies zero cross-covariance because Gaussian variables have finite second moments.

The theorem's printed wording refers to “Gaussian random vectors”; the surrounding setup explicitly assumes that the stacked vector $Z$ is Gaussian. That joint assumption is indispensable.

**Derivation.** With $u\in\mathbb R^n$ and $v\in\mathbb R^m$, zero cross-covariance removes the mixed quadratic term:

$$
\begin{aligned}
\varphi_{X,Y}(u,v)
&=\exp\!\left(iu^Tm_X-\frac12u^TP_Xu\right)
\exp\!\left(iv^Tm_Y-\frac12v^TP_Yv\right)\\
&=\varphi_X(u)\varphi_Y(v).
\end{aligned}
$$

Factorization of the joint characteristic function implies independence. This argument also covers degenerate Gaussian blocks.

**Interpretation and use.** For jointly Gaussian variables, all dependence is encoded by cross-covariances. This converts a second-order calculation into a full independence statement and enables the residual argument for Gaussian conditioning.

### 6.3 Theorem 2.11 — Affine transformations preserve Gaussianity

*Source: p. 44.*

**Statement.** If $Z\in\mathbb R^p$ is Gaussian, $C\in\mathbb R^{q\times p}$ and $a\in\mathbb R^q$ are deterministic, then

$$
Z\sim\mathcal N(m_Z,P_Z)
\implies CZ+a\sim\mathcal N(Cm_Z+a,CP_ZC^T).
$$

No invertibility of $C$ is required; the output covariance may be singular.

**Derivation.** For $W=CZ+a$,

$$
\begin{aligned}
\varphi_W(u)
&=e^{iu^Ta}\varphi_Z(C^Tu)\\
&=\exp\!\left(iu^T(Cm_Z+a)-\frac12u^TCP_ZC^Tu\right),
\end{aligned}
$$

which is the characteristic function of the stated Gaussian law.

**Interpretation and use.** Section 4's moment propagation works for general variables; this theorem additionally propagates the complete distribution within the Gaussian family. Linear Gaussian state dynamics therefore preserve the family needed for a finite parameter description. A nonlinear function of a Gaussian vector is not generally Gaussian.

### 6.4 Theorem 2.12 — Gaussian marginals

*Source: pp. 44–45.*

**Statement.** If the stacked vector $(X^T,Y^T)^T$ is jointly Gaussian, then

$$
X\sim\mathcal N(m_X,P_X),\qquad
Y\sim\mathcal N(m_Y,P_Y).
$$

More generally, every subvector of a Gaussian vector is Gaussian. A concise derivation uses a deterministic selection matrix: $Y=[\,0\ \ I_m\,]Z$, so Theorem 2.11 applies. Equivalently, set the unused arguments of the joint characteristic function to zero, as in Eq. (2.72).

**Interpretation and use.** Marginalization preserves Gaussianity and simply selects the appropriate parameter blocks. This implication runs from joint to marginal Gaussianity; the converse fails, as the counterexample in Section 6.1 demonstrates.

### 6.5 Theorem 2.13 — Conditional Gaussian distributions

*Source: p. 45, Eqs. (2.137)–(2.138); scalar precursor: Example 2.19, pp. 41–42.*

**Statement and assumptions.** Under the textbook's density-based proof, assume $Z=(X^T,Y^T)^T$ is jointly Gaussian and $P_Z\succ0$. Then $P_Y\succ0$, so $P_Y^{-1}$ exists, and

$$
X\mid(Y=y)\sim\mathcal N(m_{X\mid y},P_{X\mid y}),
$$

with

$$
\boxed{
m_{X\mid y}=m_X+P_{XY}P_Y^{-1}(y-m_Y)
}
$$

and

$$
\boxed{
P_{X\mid Y}=P_{X\mid y}
=P_X-P_{XY}P_Y^{-1}P_{YX}.
}
$$

The left notation $P_{X\mid Y}$ normally denotes a random conditional covariance. Here it is a constant matrix, independent of the observed value. The Gaussian formulas provide a natural conditional-law version for every $y$ when $P_Y\succ0$.

**Derivation using an independent Gaussian residual.** Define

$$
K=P_{XY}P_Y^{-1}\in\mathbb R^{n\times m},\qquad
R=X-m_X-K(Y-m_Y).
$$

The pair $(R,Y)$ is jointly Gaussian by Theorem 2.11, and

$$
\mathbb E[R]=0,\qquad
\operatorname{Cov}(R,Y)=P_{XY}-KP_Y=0.
$$

Theorem 2.10 therefore implies that $R$ is independent of $Y$. Expanding its covariance gives

$$
\begin{aligned}
P_R
&=P_X-KP_{YX}-P_{XY}K^T+KP_YK^T\\
&=P_X-P_{XY}P_Y^{-1}P_{YX}.
\end{aligned}
$$

Finally,

$$
X=m_X+K(Y-m_Y)+R.
$$

After fixing $Y=y$, the first two terms are deterministic, while independence leaves the Gaussian law of $R$ unchanged. Its mean and covariance give the two conditional formulas. This is a centered version of the decorrelating transformation used in the source's proofs of Theorems 2.12–2.13.

**Gain and innovation.** The innovation $y-m_Y$ is the discrepancy between the observed measurement and its prior mean. The matrix $K$ maps that discrepancy into a correction of the state estimate. Cross-covariance determines how the state relates to observation deviations, while $P_Y^{-1}$ accounts for the scale and correlation of observation uncertainty. The product has exactly the dimensions needed to turn an $m$-component measurement discrepancy into an $n$-component state correction.

**Why covariance is observation-independent.** The independent residual is the part of $X$ that cannot be inferred from $Y$. Conditioning changes only the deterministic shift in the above decomposition, so the residual covariance remains the same for every $y$. This is a property of the joint Gaussian model, not a general property of conditioning.

**Why uncertainty cannot increase in this model.** With $P_Y\succ0$,

$$
P_X-P_{X\mid Y}
=P_{XY}P_Y^{-1}P_{YX}
=(P_{XY}P_Y^{-1/2})(P_{XY}P_Y^{-1/2})^T
\succeq0.
$$

Since the conditional covariance is itself positive semidefinite,

$$
0\preceq P_{X\mid Y}\preceq P_X.
$$

The ordering means that conditional variance cannot exceed prior variance along any state direction. It does not mean that every matrix entry decreases numerically. The removed covariance also equals the covariance of the conditional mean:

$$
P_{\mathbb E[X\mid Y]}
=KP_YK^T=P_{XY}P_Y^{-1}P_{YX},
$$

which makes the law of total covariance explicit. If $P_{XY}=0$, the observation leaves the entire state law unchanged by joint Gaussian independence. Even with nonzero cross-covariance, directions unrelated to the observation may receive no reduction.

### 6.6 Singular covariance: qualify the formulas before using an inverse

The chapter explicitly notes on p. 46 that its proofs assumed $P_Z\succ0$, and that the conditional formulas can extend to singular $P_Z$. Two cases must be separated.

If **$P_Z$ is singular but $P_Y$ is invertible**, the residual proof above still works with possibly degenerate Gaussian laws. The same conditional mean and covariance formulas remain valid, although the conditional law may be concentrated on a lower-dimensional affine set and lack an ordinary $n$-dimensional density. As a supplementary illustration, $X=Y$ for a nondegenerate scalar Gaussian $Y$ gives a singular joint covariance and zero conditional variance.

If **$P_Y$ itself is singular**, the displayed inverse is undefined. One must account for exact linear relations among measurement components, for example by retaining independent observation coordinates with nonsingular covariance. Condition on values in the affine support $m_Y+\operatorname{range}(P_Y)$; the model does not determine a unique conditional law at impossible off-support observations. Generalized-inverse formulations are possible with the appropriate support treatment, but they are outside the chapter's proof and are not needed here. The note about singular $P_Z$ must not be read as permission to invert a singular $P_Y$.

These distinctions preserve the source's valid Gaussian conclusions while separating distributions from densities and keeping all inverse operations justified.

## H. Worked example — Gaussian state estimation

**Supplementary pedagogical example; not an example from the textbook.**

Suppose a hidden scalar state and an independent measurement error satisfy

$$
X\sim\mathcal N(10,4),\qquad
V\sim\mathcal N(0,1),\qquad
Y=X+V.
$$

The second parameter of each Gaussian law is its **variance**. Thus the prior state standard deviation is 2 and the measurement-noise standard deviation is 1. Independence makes $(X,V)$ jointly Gaussian. Since $(X,Y)$ is its affine transformation, Theorem 2.11 establishes the joint Gaussianity required for conditioning.

### 8.1 Build the joint model before conditioning

The prior and measurement means are

$$
m_X=10,\qquad P_X=4,\qquad
m_Y=\mathbb E[X+V]=10.
$$

Independence gives $\operatorname{Cov}(X,V)=0$, so

$$
P_Y=\operatorname{Var}(X+V)=4+1=5,
$$

$$
P_{XY}=\operatorname{Cov}(X,X+V)=4.
$$

Equivalently, Section 4's affine covariance propagation gives

$$
\begin{bmatrix}X\\Y\end{bmatrix}
=\begin{bmatrix}1&0\\1&1\end{bmatrix}
\begin{bmatrix}X\\V\end{bmatrix},\qquad
P_{X,Y}=\begin{bmatrix}4&4\\4&5\end{bmatrix}.
$$

The joint covariance is positive definite: its leading diagonal entry is positive and its determinant is $4\cdot5-4^2=4>0$.

### 8.2 Observe the measurement and update

For $Y=12$, the innovation is $12-10=2$ and the gain is

$$
K=P_{XY}P_Y^{-1}=\frac45=0.8.
$$

Theorem 2.13 gives

$$
m_{X\mid12}=10+0.8(12-10)=11.6,
$$

$$
P_{X\mid12}=4-\frac{4^2}{5}=0.8.
$$

Therefore

$$
\boxed{X\mid(Y=12)\sim\mathcal N(11.6,0.8).}
$$

The posterior mean can also be written $0.2\cdot10+0.8\cdot12$. The measurement is more precise than the prior, so it receives greater weight, but its uncertainty prevents the state estimate from simply becoming 12. The posterior standard deviation is $\sqrt{0.8}\approx0.894$, smaller than the prior value 2.

The posterior density also follows directly from Bayes' theorem:

$$
p_{X\mid Y}(x\mid12)
\propto\exp\!\left[-\frac{(x-10)^2}{8}-\frac{(12-x)^2}{2}\right].
$$

Combining the quadratic terms gives precision $1/4+1=5/4$, hence variance $4/5$, and mean $(10/4+12)/(5/4)=11.6$. This checks the conditional-Gaussian calculation using Section 5's general rule.

### 8.3 Connect the three central ideas

Section 4 constructs the joint mean and covariance from the measurement model. Section 5 identifies the posterior as the prior multiplied by a likelihood and normalized. Section 6 makes that update explicit using only Gaussian parameters. The supplementary MMSE result then identifies 11.6 as the realized minimum mean squared error estimate.

Across all possible measurements, $\mathbb E[X\mid Y]=10+0.8(Y-10)$ has variance $0.8^2\cdot5=3.2$. Thus total covariance reads

$$
4=0.8+3.2.
$$

The prior variance splits into remaining uncertainty and variability explained by measurements. In this model every possible observation has the same posterior variance 0.8, although the posterior mean moves with the observed value.

## I. Connection to filtering theory

**Supplementary forward-looking connection; Chapter 2 does not derive the recursive filtering algorithm.**

### 9.1 General Bayesian filtering: predict, then condition

Let $X_k$ be the hidden state, $Y_k$ the observation, and $y_{1:k}=(y_1,\ldots,y_k)$ the observed history. For the usual state-space recursion, assume an initial state law and the conditional-independence structure

$$
p(x_k\mid x_{0:k-1},y_{1:k-1})=p(x_k\mid x_{k-1}),
$$

$$
p(y_k\mid x_{0:k},y_{1:k-1})=p(y_k\mid x_k).
$$

The first is the Markov state assumption together with the absence of additional dependence on past observations once the preceding state is given. The second states that the current state contains all the information needed to specify the current observation law. Independent driving and measurement noises in a standard generative state-space model are sufficient to obtain this structure. The relevant densities are assumed to exist; the measurement normalization must be finite and positive.

**Prediction** marginalizes the previous state:

$$
p(x_k\mid y_{1:k-1})
=\int p(x_k\mid x_{k-1})
p(x_{k-1}\mid y_{1:k-1})\,dx_{k-1}.
$$

**Measurement update** applies Bayes' theorem:

$$
p(x_k\mid y_{1:k})
=\frac{p(y_k\mid x_k)p(x_k\mid y_{1:k-1})}
{\int p(y_k\mid\xi)p(\xi\mid y_{1:k-1})\,d\xi}.
$$

Equivalently, the posterior is proportional to likelihood times predicted prior. The prediction step uses joint laws and marginalization; the update uses conditional probability. These formulas require no Gaussian assumption. For discrete states, replace integrals with sums; more general formulations use probability kernels when ordinary densities are unavailable.

### 9.2 Why linear Gaussian structure gives the Kalman filter

Consider the model

$$
X_k=F_kX_{k-1}+b_k+W_{k-1},\qquad
Y_k=H_kX_k+d_k+V_k.
$$

Assume deterministic matrices and offsets, a Gaussian initial state, zero-mean Gaussian process noises with covariances $Q_{k-1}$, zero-mean Gaussian measurement noises with covariances $R_k$, and mutual independence of the initial state and all process and measurement noises across time and between sequences. These are sufficient standard assumptions; correlated-noise models require adjusted formulas.

Write $(m_{k-1}^+,P_{k-1}^+)$ for the posterior parameters after $y_{1:k-1}$ and $(m_k^-,P_k^-)$ for the predicted parameters. Affine Gaussian propagation gives

$$
m_k^-=F_km_{k-1}^++b_k,\qquad
P_k^-=F_kP_{k-1}^+F_k^T+Q_{k-1}.
$$

Given past observations, the predicted state and current measurement are jointly Gaussian with measurement mean, innovation covariance, and cross-covariance

$$
\widehat y_k^-=H_km_k^-+d_k,\qquad
S_k=H_kP_k^-H_k^T+R_k,\qquad
P_{X_kY_k\mid y_{1:k-1}}=P_k^-H_k^T.
$$

If $S_k\succ0$—for example, if $R_k\succ0$—Theorem 2.13 yields

$$
\begin{aligned}
K_k&=P_k^-H_k^TS_k^{-1},\\
m_k^+&=m_k^-+K_k(y_k-\widehat y_k^-),\\
P_k^+&=P_k^--P_k^-H_k^TS_k^{-1}H_kP_k^-.
\end{aligned}
$$

These are the basic Kalman prediction and update equations in exact arithmetic. Gaussianity is preserved at both stages, so the posterior is represented exactly by a finite mean vector and covariance matrix. Under squared-error loss, the resulting conditional mean is also the unrestricted MMSE estimate.

**Bayesian filtering does not require Gaussian distributions. Kalman filtering exploits additional linear-Gaussian structure to obtain a finite-dimensional exact posterior representation.** With nonlinear dynamics or non-Gaussian laws, the posterior need not remain Gaussian, and mean and covariance generally cease to describe it completely. General density-based methods or approximations, such as numerical integration, Gaussian approximations, mixtures, or particle representations, may then be needed. The chapter's universal probability identities remain valid even when Gaussian closure fails.

## J. Chapter synthesis

### 10.1 The conceptual chain

A probability space makes uncertainty mathematically well-defined. Random variables translate outcomes into quantities of interest, and distributions describe their possible values. Joint laws add dependence; moments provide selected summaries of those laws. Conditioning turns dependence into updated knowledge. The Gaussian family is especially useful because its joint structure, affine transformations, marginals, and conditional laws can all be described by means and covariances.

### 10.2 Essential formula reference

The equations in this table have been developed above; their assumptions are part of the result, not optional qualifications.

| Result                          | Compact reference                                          | Main assumptions or qualification                                         |
| ------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------- |
| Event conditioning              | $P(A\mid B)=P(A\cap B)/P(B)$                             | $P(B)>0$                                                                |
| Marginal density                | $p_X(x)=\int p_{X,Y}(x,y)\,dy$                           | Joint density exists                                                      |
| Bayes' theorem                  | $p_{X\mid Y}(x\mid y)=p_{Y\mid X}(y\mid x)p_X(x)/p_Y(y)$ | Applicable densities; $p_Y(y)>0$                                        |
| Mean of a function              | $\mathbb E[g(X)]=\int g(x)p_X(x)\,dx$                    | Density setting and absolute integrability; discrete version uses a sum   |
| Characteristic function         | $\varphi_X(u)=\mathbb E[e^{iuX}]$                        | Always exists for real $X$                                              |
| Independent sum                 | $\varphi_{X+Y}=\varphi_X\varphi_Y$                       | Independence; no moment requirement                                       |
| Affine mean and covariance      | $m_{AX+b}=Am_X+b$, $P_{AX+b}=AP_XA^T$                  | Deterministic $A,b$; finite second moments for covariance               |
| Total expectation               | $\mathbb E[X]=\mathbb E[\mathbb E[X\mid Y]]$             | Integrability                                                             |
| Total covariance                | $P_X=\mathbb E[P_{X\mid Y}]+P_{\mathbb E[X\mid Y]}$      | Finite second moments                                                     |
| MMSE estimate                   | $\widehat X=\mathbb E[X\mid Y]$                          | Finite second moment; squared-error loss; supplementary estimation result |
| Gaussian independence           | $P_{XY}=0\Rightarrow X\perp Y$                           | Joint Gaussianity; Theorem 2.10                                           |
| Gaussian affine closure         | $CZ+a\sim\mathcal N(Cm_Z+a,CP_ZC^T)$                     | Gaussian $Z$; deterministic $C,a$; Theorem 2.11                       |
| Gaussian conditional mean       | $m_{X\mid y}=m_X+P_{XY}P_Y^{-1}(y-m_Y)$                  | Joint Gaussianity and invertible $P_Y$; Theorem 2.13                    |
| Gaussian conditional covariance | $P_{X\mid Y}=P_X-P_{XY}P_Y^{-1}P_{YX}$                   | Same assumptions; possibly degenerate conditional law                     |

### 10.3 Concept-to-filtering map

| Chapter 2 concept                          | Role in filtering                                                               |
| ------------------------------------------ | ------------------------------------------------------------------------------- |
| Probability space and measurable variables | Define states, disturbances, and observations consistently                      |
| Joint distribution                         | Encodes how hidden states and observations are related                          |
| Marginalization and transformed densities  | Propagate uncertainty through dynamics and eliminate unobserved variables       |
| Mean, covariance, cross-covariance         | Summarize location, directional uncertainty, and state–measurement association |
| Bayes' theorem                             | Converts predicted uncertainty and a measurement likelihood into a posterior    |
| Conditional expectation                    | Produces the posterior mean and, under squared error, the optimal estimate      |
| Total covariance                           | Separates remaining uncertainty from variation explained by observations        |
| Gaussian affine closure and conditioning   | Give exact finite parameter prediction and update in a linear Gaussian model    |

### 10.4 Long-term insights and common misunderstandings

1. **A probability model and a physical system are different objects.** Correct deductions depend on the assigned model being appropriate for the intended prediction.
2. **Distributions contain more information than moments.** Mean and covariance characterize Gaussian laws, but generally leave much of a non-Gaussian law unspecified.
3. **Almost-sure equality allows exceptional outcomes.** Probability zero is not the same as impossibility at every point of the sample space.
4. **Uncorrelatedness is weaker than independence.** Joint Gaussianity is the special assumption that makes zero cross-covariance sufficient for independence.
5. **Gaussian marginals do not imply a Gaussian joint law.** Gaussian conditioning must be justified from the joint model.
6. **Moment propagation and distributional closure are distinct.** Affine mean and covariance formulas are general; affine Gaussian preservation additionally identifies the complete law.
7. **Conditional means and covariances are generally random functions of observations.** A constant conditional covariance is a special Gaussian feature, while average covariance reduction follows more generally.
8. **Estimator optimality depends on the loss and admissible information.** Conditional expectation minimizes squared error among functions of the observation; it need not minimize a different loss.
9. **An inverse requires nonsingularity.** Degenerate Gaussian distributions remain valid, but their densities and conditioning formulas require the qualifications in Section 6.6.

**Chapter 2 provides the mathematical foundation for representing uncertainty, describing statistical dependence, and updating probabilistic knowledge through observations.**

---

## Presentation Knowledge Map

This independently readable map organizes eight conceptual themes for a future academic presentation. It does not prescribe a slide count. Theorem numbers refer to Chapter 2 of Jazwinski's *Stochastic Processes and Filtering Theory*. MMSE optimality, the numerical example, and recursive filtering are explicitly supplementary. In the equations below, $m_X=\mathbb E[X]$, $P_X=\operatorname{Cov}(X)$, and $P_{XY}=\operatorname{Cov}(X,Y)$; vectors are columns.

### Theme 1 — Probability models and measurable uncertainty

- **Central mathematical result:** A model is a triple $(\Omega,\mathcal B,P)$ with countable additivity: $P(\bigcup_j A_j)=\sum_jP(A_j)$ for disjoint measurable events.
- **Key takeaway:** Probability theory derives consequences of an assigned model; a correct deduction does not validate the model's physical assumptions. Almost-sure statements may have exceptional null sets.
- **Filtering relevance:** The state, noise, and observation models must describe the uncertainty and dependence that matter in practice.
- **Suggested visual:** Physical system → probability model → inference → empirical assessment, with the three stages of Section 2.1 labeled.

### Theme 2 — Distributions, moments, and Fourier representations

- **Central equation:** $\varphi_X(u)=\mathbb E[e^{iuX}]$; for $X\sim\mathcal N(m,\sigma^2)$, $\varphi_X(u)=\exp(ium-\sigma^2u^2/2)$.
- **Key takeaway:** A characteristic function always exists and determines a distribution. Moments require integrability; mean and variance identify a law only within special families such as the Gaussian family.
- **Filtering relevance:** Distribution representations explain why Gaussian uncertainty is tractable and why non-Gaussian uncertainty may require more than two moments.
- **Suggested visual:** Compare distributions with the same mean and variance, followed by the Gaussian density and its compact characteristic function.

### Theme 3 — Dependence, covariance, and Gaussian independence

- **Central mathematical result:** For jointly Gaussian $X,Y$, $P_{XY}=0$ implies $X\perp Y$ (Theorem 2.10).
- **Key takeaway:** Independence implies zero covariance when second moments exist, but the converse needs joint Gaussianity. Separate Gaussian marginals do not suffice.
- **Filtering relevance:** State–measurement cross-covariance determines how observation discrepancies can correct a state estimate.
- **Suggested visual:** Contrast a dependent uncorrelated pair on a circle with a jointly Gaussian pair having zero cross-covariance.

### Theme 4 — Affine propagation of uncertainty

- **Central equation:** For deterministic $A,b$ and finite-second-moment $X$, $Y=AX+b$ implies $m_Y=Am_X+b$ and $P_Y=AP_XA^T$. If $X$ is Gaussian, $Y$ is Gaussian (Theorem 2.11).
- **Key takeaway:** Covariance transforms geometrically under a linear map, while a deterministic translation changes only the mean. Gaussian closure is additional to the general moment identities.
- **Filtering relevance:** These results produce the mean and covariance prediction step of a linear Gaussian filter.
- **Suggested visual:** An uncertainty ellipse translated, rotated, stretched, and projected by an affine map; identify any collapsed direction.

### Theme 5 — Bayesian conditioning and the meaning of an estimate

- **Central equation:** $p_{X\mid Y}(x\mid y)=p_{Y\mid X}(y\mid x)p_X(x)/p_Y(y)$ for applicable densities with $p_Y(y)>0$.
- **Key takeaway:** The posterior is a complete updated law. Its mean $g(y)=\mathbb E[X\mid Y=y]$ is a numerical estimate, while $g(Y)$ is a random estimator before observation.
- **Filtering relevance:** Every measurement update combines the predicted prior with a likelihood and normalizes.
- **Suggested visual:** Prior and likelihood curves producing a normalized posterior, with the posterior mean marked separately from the full density.

### Theme 6 — Remaining uncertainty and squared-error optimality

- **Central mathematical result:** $P_X=\mathbb E[P_{X\mid Y}]+P_{\mathbb E[X\mid Y]}$ (Example 2.17); under finite second moments, the supplementary MMSE decomposition is $J(a)=J(g)+\mathbb E[\|g(Y)-a(Y)\|^2]$ with $g(Y)=\mathbb E[X\mid Y]$.
- **Key takeaway:** Observations explain part of prior variability; conditional expectation minimizes expected squared error. General covariance reduction is an average statement, not a guarantee at every observation.
- **Filtering relevance:** The conditional mean is the estimation target under squared error, and conditional covariance quantifies the error that remains.
- **Suggested visual:** A decomposition of prior uncertainty into explained variation and average remaining uncertainty, alongside perpendicular error components.

### Theme 7 — Gaussian conditioning as an innovation correction

- **Central equation:** For jointly Gaussian $X,Y$ and $P_Y\succ0$, $m_{X\mid y}=m_X+K(y-m_Y)$ and $P_{X\mid Y}=P_X-KP_YK^T$, where $K=P_{XY}P_Y^{-1}$ (Theorem 2.13).
- **Key takeaway:** The innovation moves the mean; an independent Gaussian residual fixes the remaining covariance, which is independent of $y$ and satisfies $0\preceq P_{X\mid Y}\preceq P_X$. Singular observation covariance requires support reduction or a qualified alternative to inversion.
- **Filtering relevance:** This is the mathematical structure of the Kalman measurement update.
- **Suggested visual:** A joint Gaussian ellipse, a slice at an observed measurement, and the state mean correction. Annotate the supplementary example $X\sim\mathcal N(10,4)$, $Y=X+V$, $V\sim\mathcal N(0,1)$ independent, $y=12$, giving posterior $\mathcal N(11.6,0.8)$.

### Theme 8 — From Bayesian recursion to the Kalman filter

- **Central equation:** Under Markov state dynamics and state-conditioned observations, $p(x_k\mid y_{1:k})\propto p(y_k\mid x_k)\int p(x_k\mid x_{k-1})p(x_{k-1}\mid y_{1:k-1})\,dx_{k-1}$.
- **Key takeaway:** Prediction and conditioning are general probabilistic operations. A Gaussian initial law, affine dynamics and measurements, and independent Gaussian noises make the posterior exactly Gaussian and finitely parameterized; other models need not have this closure.
- **Filtering relevance:** This supplementary connection assembles Chapter 2's marginalization, affine propagation, and conditioning into recursive state estimation.
- **Suggested visual:** A prediction–update cycle carrying a full density in the general case and the pair $(m_k,P_k)$ in the linear Gaussian case.
