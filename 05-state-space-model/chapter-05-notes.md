Chapter 5: State Space Model
================

## 5.1 Stochastic Model

- State space models are not deterministic — observations are assumed to
  be samples that are obtained from a probability distribution.
- See Fig 5.1 on page 60 for an example with a 3d plot of the Nile
  flows.

## 5.2 Definition of State-Space Model

- State space models introduce *latent* variables that are not directly
  observed and inform the observations.
- The latent variables are *states*.
- States have the assumption of having *Markov properties*, i.e., *a
  state is related only to the state at the previous time point.*
- Additionally, *an observation at a certain time point depends only on
  the state at the same time point.*

> Mark note: contrast this with, say, the GP used to model NPS over
> time. The GP modeled the latent satisfaction that then informed the
> observed response, but it wasn’t a state-space model. The nature of GP
> regression is that each point has some degree of correlation with
> latent states *beyond* the most recent state.

### 5.2.1 Representation by Graphical Model

- See figure 5.2 on page 61 for a DAG representation of the state space
  model.
- $x_0$ is the prior for the state. $x_t$ can be a $p$-dimensional
  column vector if there are $p$ elements in the state.

### 5.2.2 Representation by Probability Distribution

- We can also represent the probability of an observation $y_t$ at a
  point in time $t$ given a state $x_t$. The probability of $x_t$ is
  conditional on the previous state, $x_{t-1}$.

$$
\begin{align*}
p(x_t | x_{0:t-1}, y_{1:t-1}) & = p(x_t | x_{t-1}) \\
p(y_t | x_{0:t}, y_{1:t-1}) & = p(y_t | x_t)
\end{align*}
$$

- This is just another way of writing the DAG. I.e., $y_t$ only depends
  on $x_t$ and $x_t$ only depends on $x_{t-1}$
- See figures 5.3 & 5.4 for more DAG representations.

### 5.2.3 Representation by Equation

- Another way of representing state space models:

$$
\begin{align*}
x_t &= f(x_{t-1}, w_t) \\
y_t &= h(x_t, v_t)
\end{align*}
$$

- $f$ and $h$ are arbitrary functions. $w$ and $v$ are white noise —
  state noise and observation noise, respectively.

### 5.2.4 Joint Distribution of State-Space Model

- See pages 63-65 for the full derivation of the joint distribution, but
  here’s the final outcome:

$$
p(\text{all random variables}) = p(x_0) \prod_{t=1}^T p(y_t\ |\ x_t)\ p(x_t\ |\ x_{t-1})
$$

## 5.3 Features of State-Space Model

- The latent state enables the state space model to be used as the basis
  for easily constructing a complicated model by combining multiple
  states.
- The state-space model represents the relation among observations via
  states indirectly rather than by direct connection among observations.
  - Contrast this with an *ARMA* model (autoregressive moving average),
    which conditions observations based on relation to *other
    observations*.
  - Hagiwara notes that an ARIMA model can be defined as one of the
    sate-space models, but a pure state-space modeling approach is
    preferred for its flexibility and explicitness in modeling approach.

## 5.4 Classification of State-Space Models

- The equations in section 5.2.3 can be used to further subdefine the
  general state-space model:

- If both $f$ and $h$ are linear functions and both $w$ and $v$ have
  Gaussian distributions, the state space model is referred to as a
  *linear Gaussian state-space model* and can be represented as follows:

$$
\begin{align*}
x_t &= G_tx_{t-1} + w_t \\
y_t &= F_tx_t + v_t \\
w_t &\sim \text{MVNormal}(0, W_t) \\
v_t &\sim \text{Normal}(0, V_t)
\end{align*}
$$

- $G_t$ is a $p \times p$ state transition matrix and $F_t$ is a
  $1 \times p$ observation matrix.
- $W_t$ is a $p \times p$ covariance matrix for the state noise and
  $V_t$ is a variance for the observation noise.
- The prior, $x_0$ can be expressed as
  $x_0 \sim \text{MVNormal}(m_0, C_0)$, where $m_0$ is a $p$-dimensional
  mean vector and $C_0$ is a $p \times p$ covariance matrix.
- Alternatively, we can express the equations with probability
  distributions:

$$
\begin{align*}
x_t &\sim \text{MVNormal}(G_tx_{t-1}, W_t) \\
y_t &\sim \text{Normal}(F_tx_t, V_t)
\end{align*}
$$

- This gives a set of parameters for the linear Gaussian state-space
  model:
- $\theta = \{G_t, F_t, W_t, V_t, m_0, C_0\}$
- We can also refer to this as a *dynamic linear model* (DLM). It’s
  super flexible dawg.
