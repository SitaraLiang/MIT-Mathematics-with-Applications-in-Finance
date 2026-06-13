
We've moved from simulating random paths to the **Deterministic PDE** (Partial Differential Equation) that prices almost every financial contract in existence. Think of this like **Gradient Descent**. In AI, we move in the direction that minimizes error. In Black-Scholes, we move in the direction that **eliminates risk**.

## Black-Scholes Differential Equation

- $G$ (or sometimes $f$ or $C/P$): Represents the **Derivative (the Option)**. Its value is "contingent" on the stock, meaning its price is a function of the stock price and time: $G(t, P_t)$.
- $P$ (or $S$ in many textbooks): Represents the **Underlying Asset** (the Stock). It follows the Geometric Brownian Motion (the "random" part of the model).

- $\{P_t, t≥0\}$ asset price follows geometric Brownian Motion: $dP_t = P_t\mu dt+\sigma P_tdB_t$ 
- $\{G_t = G(t, P_t),  t≥0\}$ price of derivative contingent on $P_T$ (e.g., call option). By Ito's lemma: $dG = \left[\frac{\partial G}{\partial P} \mu P_t + \frac{\partial G}{\partial t} + \frac{1}{2} \frac{\partial^2 G}{\partial P^2}\sigma^2 P_t^2\right] dt +  \frac{\partial G}{\partial P} \sigma P_tdB_t$

<u>Problem</u>: Both the stock price $P_t$ and the option price $G_t$ are "noisy" because they both contain the Brownian Motion term $\Delta B_t$.
<u>Solution</u>: If we buy exactly $\frac{\partial G}{\partial P}$ shares of stock (this is called **Delta**), the "wiggles" in the stock perfectly cancel out the "wiggles" in the option:
Short (sell) one derivate and Buy $\frac {\partial G} {\partial P}$ shares of asset, portfolio value $V_t$ is: 

$$\begin{align}V_t & = -G_t + \frac {\partial G} {\partial P}(t, P_t)P_t \\ \\
\Delta V_t & = \left(-\frac {\partial G} {\partial P}- \frac 1 2 \frac {\partial^2 G} {\partial P^2} \sigma^2 P_t^2\right)\Delta t
\end{align}
$$
<u>Result</u>: We have a portfolio $V_t$ that has **no noise**. 

If a portfolio has no noise, it is "risk-free." In a world without arbitrage, a risk-free portfolio _must_ earn exactly the same interest rate as a bank account ($r$). If it earned more, everyone would do it until the price dropped. If it earned less, no one would do it.

$$\Delta V_t = V_t \times (r\Delta t)$$
- $V_t$: initial value
- $r(\Delta t)$: rate of return of the risk-less asset over the period $\Delta t$

If we equating the "Math side" (Itô's Lemma) with the "Finance side" (Risk-free rate), we get the PDE: 

$$\begin{align}
& \left(-\frac {\partial G} {\partial P}- \frac 1 2 \frac {\partial^2 G} {\partial P^2} \sigma^2 P_t^2\right)\Delta t = V_t \times (r\Delta t) \\ \\ 
& \frac{\partial G}{\partial t} + rP_t \frac{\partial G}{\partial P} + \frac{1}{2} \sigma^2 P_t^2 \frac{\partial^2 G}{\partial P^2} = rG_t
\end{align}
$$
Think of this equation as a **Balance Sheet** for an option:

1. **$\frac{\partial G}{\partial t}$ (Theta):** How much value the option loses just because time is passing. The option you sold loses value as it gets closer to expiring. Since you _sold_ it, this is actually **good for you** (you owe the client less money).
2. **$rP_t \frac{\partial G}{\partial P}$:** The return you get from the delta-hedged stock position.  
- $r$ is the interest rate. $P$ is the **Stock Price**.
- $\frac{\partial G}{\partial P}$ is the **Delta** (how many shares of stock you are holding).
- So, $rP \frac{\partial G}{\partial P}$ is literally just the interest the bank pays on the money it borrowed to buy the stocks ($P$) used to hedge the option ($G$).

3. **$\frac{1}{2} \sigma^2 P_t^2 \frac{\partial^2 G}{\partial P^2}$ (Gamma):** The "bonus" value you get because the stock moves randomly (volatility). This is the "convexity" term from Itô's Lemma.
4. **$rG_t$:** The cost of the capital you used to buy the option.
$$\underbrace{\frac{\partial G}{\partial t} + \frac{1}{2} \sigma^2 P^2 \frac{\partial^2 G}{\partial P^2}}_{\text{Total "Return" of the Option}} = \underbrace{rG_t - rP \frac{\partial G}{\partial P}}_{\text{The "Cost" of holding the position}}$$
- **Theta is negative** (Time decay hurts the option holder).
- **Gamma is positive** (Volatility helps the option holder).

| **Term**                                                               | **Bank's Experience (Short G)**     | **Client's Experience (Long G)** |
| ---------------------------------------------------------------------- | ----------------------------------- | -------------------------------- |
| Theta ($\frac{\partial G}{\partial t}$)                                | Positive Income (Liability shrinks) | Negative Cost (Asset decays)     |
| Gamma ($\frac{1}{2} \sigma^2 P_t^2 \frac{\partial^2 G}{\partial P^2}$) | Negative Cost (Hedge slippage)      | Positive Bonus (Convexity gain)  |

**Boundary Conditions (The "Terminal" State)**
Think of it as the "Label" in a supervised learning problem.
- At time $T$ (expiration), we know exactly what the option is worth. For a Call, it's $\max(P_T - K, 0)$.
	- T is the time to expiration
	- K is strike price
- We then work **backward** in time from $T$ to today ($t=0$) using the PDE to find out what the price should be right now.


## Heat Equation

The **Heat Equation** (also widely known as the **Diffusion Equation**) is a partial differential equation (PDE) that describes how a quantity like heat or chemical concentration diffuses through a medium over time.

Let $u(x, t) : \mathbb{R} \times \mathbb{R}^+ \rightarrow \mathbb{R}$ represent the temperature at a spatial position $x$ and time $t$. The governing 1D equation is:

$$\frac{\partial u}{\partial t} = \lambda \frac{\partial^2 u}{\partial x^2}$$

- **$\frac{\partial u}{\partial t}$**: The rate of change of temperature over time.
- **$\frac{\partial^2 u}{\partial x^2}$**: The curvature (concavity) of the temperature profile across space.
- **$\lambda$**: The thermal diffusivity constant ($\lambda > 0$).

> **Physical Example:** A long, thin, perfectly insulated metal bar. Because the sides are insulated, heat can only flow left or right along the $x$-axis.
> 
> **The Goal:** Solve the **Initial Value Problem (IVP)**, finding $u(x,t)$ given an initial temperature distribution at $t = 0$:
> $$u(x, 0) = u_0(x), \quad \text{for } -\infty < x < \infty$$

#### Fundamental Properties of Solutions

**Linearity (Superposition Principle)**
Because the heat equation is linear, it allows us to build complex solutions out of simpler ones:
- Discrete Superposition: If $u_1(x, t)$ and $u_2(x, t)$ are solutions, then their sum $u_1(x, t) + u_2(x, t)$ is also a solution.
- Continuous Superposition: If we have a family of solutions $u_s(x, t)$ indexed by a continuous variable $s \in \mathbb{R}$, then weighting them by a function $c(s)$ and integrating yields another valid solution:
$$u(x,t) = \int_{-\infty}^{\infty} u_s(x, t) \cdot c(s) \, ds$$

> Instead of solving a highly complex initial profile $u_0(x)$ directly, break $u_0(x)$ down into an infinite sum of simple localized impulses, solve those individual impulses, and integrate them back together.


#### The Fundamental Solution (The Simple Case)

To execute the strategy above, we start with the simplest possible localized impulse: a **Dirac delta function** $\delta(x)$ centered at the origin.

$$u(x, 0) = \delta(x) \quad \text{where } \delta(x) = 0 \text{ for } x \neq 0, \text{ and } \int_{-\infty}^{\infty} \delta(x) \, dx = 1$$

The solution to this specific initial value problem is called the **Fundamental Solution** (or Heat Kernel), denoted as $u_\delta(x, t)$:

$$u_\delta(x, t) = \frac{1}{\sqrt{2\pi \cdot (2\lambda t)}} e^{-\frac{x^2}{2(2\lambda t)}}$$

**Statistical Connection**
Notice that $u_\delta(x, t)$ matches the exact mathematical form of a standard Normal (Gaussian) Distribution Probability Density Function (PDF):

$$\mathcal{N}(\mu, \sigma^2) \implies \mu = 0, \quad \sigma^2 = 2\lambda t$$

- As $t \to 0$, the curve squashes into an infinitely sharp spike ($u_\delta \to \delta(x)$).
- As $t$ increases, the heat spreads out, acting exactly like a random walk or diffusion process with a variance that grows linearly with time ($2\lambda t$).
- Convenient Scale ($\lambda = \frac{1}{2}$): If we choose or scale coordinates such that $\lambda = \frac{1}{2}$, the variance simplifies perfectly to $\sigma^2 = t$, giving us:

$$u_\delta(x, t) = \frac{1}{\sqrt{2\pi t}} e^{-\frac{x^2}{2t}}$$

#### General Initial Conditions & The Probabilistic Link

Using the linearity property, we can solve the equation for **any** arbitrary initial profile $u_0(x)$ by treating $u_0(x)$ as a combination of shifted delta functions: $u_0(x) = \int_{-\infty}^{\infty} \delta(x - s)u_0(s) \, ds$.

For the scale $\lambda = \frac{1}{2}$, the general solution becomes the convolution of the initial state with our heat kernel:

$$u(x, t) = \int_{-\infty}^{\infty} u_\delta(x - s, t) \cdot u_0(s) \, ds = \int_{-\infty}^{\infty} \frac{1}{\sqrt{2\pi t}} e^{-\frac{(x - s)^2}{2t}} u_0(s) \, ds$$

**The Brownian Motion Interpretation**
Suppose the initial condition is an indicator function: $u_0(x) = \mathbf{1}(a < x < b)$ (meaning the temperature is exactly $1$ between $a$ and $b$, and $0$ everywhere else).
The integral can be interpreted through Brownian Motion $\{X_t, t \ge 0\}$ starting at position $x$:

$$X_t = x + B_t \quad \implies \quad X_t \mid X_0 = x \sim \mathcal{N}(x, t)$$

Thus, the temperature at position $x$ at time $t$ is equivalent to the expectation that a particle undergoing random molecular collisions (Brownian motion) starting at $x$ will land inside the interval $(a, b)$ at time $t$:

$$u(x, t) = \mathbb{E}_{X_t}[\mathbf{1}(a < X_t < b)] = P(a < X_t < b \mid X_0 = x)$$

#### Solution by Similarity (Invariance)

Another powerful way to solve the heat equation is by exploiting its scaling symmetries.

**Step 1: Scale Transformation**

Let's stretch space and time using constants $\alpha$ and $\beta$:

$$\tau = \alpha t, \quad y = \beta x$$

Define a new function $v(y, \tau) = u(x, t)$. Using the chain rule:

$$\frac{\partial u}{\partial t} = \alpha \frac{\partial v}{\partial \tau}, \quad \frac{\partial u}{\partial x} = \beta \frac{\partial v}{\partial y}, \quad \frac{\partial^2 u}{\partial x^2} = \beta^2 \frac{\partial^2 v}{\partial y^2}$$

Substituting these back into the original PDE ($\frac{\partial u}{\partial t} = \lambda \frac{\partial^2 u}{\partial x^2}$):

$$\alpha \frac{\partial v}{\partial \tau} = \beta^2 \lambda \frac{\partial^2 v}{\partial y^2}$$

If we pick scaling factors such that $\alpha = \beta^2$, then $\frac{\partial v}{\partial \tau} = \lambda \frac{\partial^2 v}{\partial y^2}$, which means the equation remains completely unchanged. This implies that if time scales by $\alpha$, space scales by $\sqrt{\alpha}$.

**Step 2: Similarity Variable**
Because of this scaling law, we can look for a solution that depends only on the ratio of space to the square root of time. Let $\alpha = \frac{1}{t}$. This allows us to convert a two-variable PDE into a single-variable Ordinary Differential Equation (ODE):

$$u(x, t) = h\left(\frac{x}{\sqrt{t}}\right)$$

Let $y = \frac{x}{\sqrt{t}}$. Differentiating $u(x,t) = h(y)$ with respect to $t$ and $x$:

$$\frac{\partial u}{\partial t} = -\frac{x}{2t^{3/2}} h'(y)$$

$$\frac{\partial^2 u}{\partial x^2} = \frac{1}{t} h''(y)$$

Substitute these into the heat equation:

$$-\frac{x}{2t^{3/2}} h'(y) = \lambda \frac{1}{t} h''(y) \implies -\frac{y}{2t} h'(y) = \frac{\lambda}{t} h''(y)$$

Cancel out $t$ to get a pure ODE in terms of $y$:

$$h''(y) = -\frac{y}{2\lambda} h'(y) \implies \frac{h''(y)}{h'(y)} = -\frac{y}{2\lambda}$$

**Step 3: Integration**
1. Rewrite using logarithms: $\left[\log h'(y)\right]' = -\frac{y}{2\lambda}$
2. Integrate once: $\log h'(y) = -\frac{y^2}{4\lambda} + C \implies h'(y) = c e^{-\frac{y^2}{4\lambda}}$
3. Integrate a second time: $h(y) = \int c e^{-\frac{y^2}{4\lambda}} \, dy$

This integration traces back to the Cumulative Distribution Function (CDF) of a Gaussian distribution, denoted as $\Phi$:

$$h(y) = \Phi\left(\frac{y}{\sqrt{2\lambda}}\right)$$

Converting our similarity variable $y = \frac{x}{\sqrt{t}}$ back to original terms yields:

$$u(x, t) = \Phi\left(\frac{x}{\sqrt{2\lambda t}}\right)$$

**Physical Interpretation of This Specific Solution**
This solution models a step-function initial state. If we evaluate the limit as $t \to 0$:

- If $x > 0$: $\lim_{t \to 0} \Phi(\infty) = 1$
- If $x = 0$: $\Phi(0) = \frac{1}{2}$
- If $x < 0$: $\lim_{t \to 0} \Phi(-\infty) = 0$

This matches an physical setup where half of the infinite bar ($x>0$) starts perfectly hot ($1$), and the other half ($x<0$) starts perfectly cold ($0$). Over time, the sharp boundary smoothly diffuses out following a Gaussian CDF profile.


## Stochastic Differential Equations (SDE)

An SDE defines a continuous-time stochastic process $\{X(t), t \geq 0\}$ by updating its state using a deterministic trend and a random noise component:

$$dX(t) = \mu(t, X(t))dt + \sigma(t, X(t))dB(t)$$

- **$\mu(t, X(t))$ (Drift Coefficient):** The deterministic, local rate of change. Equivalent to the velocity vector in physics or standard gradient steps.
- **$\sigma(t, X(t))$ (Diffusion Coefficient):** The volatility or noise amplitude. It scales the random perturbations.
- **$dB(t)$:** The increment of a standard Brownian Motion. We can think of $dB(t)$ as an infinitesimal Gaussian random variable: $dB(t) \sim \mathcal{N}(0, dt)$.

#### Integral Form

Because Brownian motion paths are nowhere differentiable in the classical sense, the differential form $dX(t)$ is shorthand for the integral equation:

$$X(t) = X(0) + \int_0^t \mu(s, X(s))ds + \int_0^t \sigma(s, X(s))dB(s)$$

The first integral is a standard Riemann/Lebesgue integral. The second is an **Itô Stochastic Integral**, which requires special calculus (Itô's Lemma) because $dB(s)$ has infinite total variation.

#### Theorem: Existence and Uniqueness

Just like Picard's theorem for standard ODEs, a unique solution is guaranteed if $\mu$ and $\sigma$ do not explode and are sufficiently smooth. For $t \in [0, T]$ and $X(0) = x_0$:

1. **Lipschitz Condition:** Prevent sudden structural jumps.
    $$|\mu(t, x) - \mu(t, y)|^2 + |\sigma(t, x) - \sigma(t, y)|^2 \leq K|x - y|^2$$
    
2. **Spatial Growth Condition:** Prevent the functions from blowing up to infinity.
    
    $$|\mu(t, x)|^2 + |\sigma(t, x)|^2 \leq K(1 + |x|^2)$$
    

If these hold, a unique, continuous solution exists in the $L^2$ space ($\sup_{0 \leq t \leq T} \mathbb{E}[X(t)^2] < \infty$). **Uniqueness** means that if two continuous processes solve the SDE, their paths are identical with probability 1: $P(X_t = Y_t, \, \forall t \in [0, T]) = 1$.


#### Solution Technique: Coefficient Matching via Itô's Lemma

In ML, we change variables using the Jacobian. In stochastic calculus, changing variables requires **Itô's Lemma**, which includes a second-order Taylor expansion term because $(dB(t))^2 = dt$.

If $X(t)$ follows an SDE, the differential of a smooth function $f(t, X_t)$ is:

$$df(t, X_t) = \left( \frac{\partial f}{\partial t} + \mu X_t \frac{\partial f}{\partial x} + \frac{1}{2}\sigma^2 X_t^2 \frac{\partial^2 f}{\partial x^2} \right) dt + \sigma X_t \frac{\partial f}{\partial x} dB(t)$$

**Application: Geometric Brownian Motion (GBM)**
GBM is the baseline model for asset prices in the **Black-Scholes framework**:

$$dX(t) = \mu X(t)dt + \sigma X(t)dB(t), \quad X(0) = x_0 > 0$$

To solve this, we guess a solution template $X(t) = f(t, B(t))$. Applying Itô's Lemma to $f(t, B(t))$ where the underlying process is just $B(t)$ (meaning drift $= 0$, volatility $= 1$):

$$dX(t) = \left( \frac{\partial f}{\partial t} + \frac{1}{2}\frac{\partial^2 f}{\partial x^2} \right)dt + \frac{\partial f}{\partial x}dB(t)$$

Now, match the coefficients with our original GBM definition ($dX(t) = \mu f dt + \sigma f dB(t)$):

1. $\sigma f = \frac{\partial f}{\partial x} \implies f(t,x) = e^{\sigma x + g(t)}$
2. $\mu f = \frac{\partial f}{\partial t} + \frac{1}{2}\frac{\partial^2 f}{\partial x^2}$

Substitute $f(t,x) = e^{\sigma x + g(t)}$ into the second equation:

$$\mu f = g'(t)f + \frac{1}{2}\sigma^2 f \implies g'(t) = \mu - \frac{\sigma^2}{2} \implies g(t) = \left(\mu - \frac{\sigma^2}{2}\right)t + \ln(x_0)$$

This yields the analytical solution for asset prices in Black-Scholes:

$$X(t) = x_0 e^{\left(\mu - \frac{\sigma^2}{2}\right)t + \sigma B(t)}$$

**Distribution Properties & The Growth Paradox**
Because the exponent contains a linear transformation of the Gaussian variable $B(t)$, $X(t)$ follows a Lognormal Distribution:

$$X(t) \sim \text{Lognormal}(\mu^*, \sigma^*) \quad \text{where } \mu^* = \left(\mu - \frac{\sigma^2}{2}\right)t, \ \sigma^* = \sigma\sqrt{t}$$

- **Expected Value:** $\mathbb{E}[X(t)] = x_0 e^{\mu t}$

> **The GBM Paradox:** If $0 < \mu < \frac{\sigma^2}{2}$, then as $t \to \infty$:
> 
> $$\mathbb{E}[X(t)] \to \infty \quad \text{and yet} \quad P(X(t) < c) \to 1 \quad \text{for any constant } c > 0$$
> 
> _ML Intuition:_ This is a consequence of extreme skewness. In a few rare paths, the asset price explodes to astronomical values, pushing the _average_ (expectation) to infinity. However, in the vast majority of simulated paths (probability approaching 1), the drag term $-\frac{\sigma^2}{2}$ dominates, causing the asset price to decay toward 0.


#### Mean-Reverting Processes: Ornstein-Uhlenbeck (OU)

The Ornstein-Uhlenbeck process introduces a restorative force. In quantitative finance, it is used to model variables that cannot grow indefinitely, such as interest rates (the Vasicek model) or volatility asset dynamics (crucial for the Heston Model project).

$$dX_t = -\alpha(X_t - \mu)dt + \sigma dB_t$$

- **$-\alpha(X_t - \mu)$ (Mean-Reverting Drift):** Acts like a spring force (Hooke's Law).
    
    - If $X_t > \mu$, the drift becomes negative, pulling the process back down.
        
    - If $X_t < \mu$, the drift becomes positive, pushing the process back up.
        
    - $\alpha$ determines the speed of reversion, and $\mu$ is the long-term equilibrium mean.
        


**Solving the OU SDE (Product-Process/Integrating Factor)**
For simplicity, set $\mu = 0 \implies dX_t = -\alpha X_t dt + \sigma dB_t$.
We guess a product solution structure: $X_t = a(t)\left[x_0 + \int_0^t b(s)dB_s\right]$. Taking the differential:

$$dX_t = a'(t)\left[x_0 + \int_0^t b(s)dB_s\right]dt + a(t)b(t)dB_t = \frac{a'(t)}{a(t)}X_t dt + a(t)b(t)dB_t$$

Matching coefficients with our SDE ($-\alpha X_t dt + \sigma dB_t$):
1. $\frac{a'(t)}{a(t)} = - \alpha \implies a(t) = e^{-\alpha t}$ (given $a(0)=1$)
2. $a(t)b(t) = \sigma \implies b(t) = \sigma e^{\alpha t}$

Substituting these back provides the analytical solution for an OU process:

$$X_t = x_0 e^{-\alpha t} + \sigma \int_0^t e^{-\alpha(t-s)} dB_s$$

_(If $\mu \neq 0$, the solution shifts to: $X_t = \mu + (x_0 - \mu)e^{-\alpha t} + \sigma \int_0^t e^{-\alpha(t-s)} dB_s$)_

**Asymptotic Properties (Stationary Distribution)**
Using Itô Isometry to calculate the variance of the stochastic integral component:
- **Expected Value:** $\mathbb{E}[X_t] = \mu + (x_0 - \mu)e^{-\alpha t} \xrightarrow{t \to \infty} \mu$
- **Variance:** $\text{Var}[X_t] = \sigma^2 \int_0^t e^{-2\alpha(t-s)}ds = \frac{\sigma^2}{2\alpha}\left(1 - e^{-\2\alpha t}\right) \xrightarrow{t \to \infty} \frac{\sigma^2}{2\alpha}$

As $t \to \infty$, the process reaches a stationary state, settling into a permanent normal distribution: $X_\infty \sim \mathcal{N}\left(\mu, \frac{\sigma^2}{2\alpha}\right)$.


#### Multi-Dimensional Systems & Structural Connections

In complex setups, multiple SDEs run simultaneously. We write them compactly using vector-matrix notation:

$$d\vec{X}_t = \vec{\mu}(t, \vec{X}_t)dt + \mathbf{\sigma}(t, \vec{X}_t)d\vec{B}_t$$

Where $\mathbf{\sigma}$ is an $m \times m$ volatility matrix, and $d\vec{B}_t$ is a vector of independent Brownian motions.

**Connection to the Project (Heston vs. Black-Scholes)**

| **Feature**                  | **Black-Scholes Framework**                 | **Heston Stochastic Volatility Model**                      |
| ---------------------------- | ------------------------------------------- | ----------------------------------------------------------- |
| **Asset Price SDE ($dS_t$)** | $dS_t = \mu S_t dt + \sigma S_t dB_t^{(1)}$ | $dS_t = \mu S_t dt + \sqrt{V_t} S_t dB_t^{(1)}$             |
| **Volatility Structure**     | Constant Parameter ($\sigma$)               | Dynamically varies over time via an SDE ($V_t$)             |
| **Volatility SDE ($dV_t$)**  | _None_                                      | $dV_t = \kappa(\theta - V_t)dt + \xi \sqrt{V_t} dB_t^{(2)}$ |
| **SDE System Type**          | 1D Scalar SDE                               | 2D Vector SDE System (Coupled)                              |

- **Heston Structure Explained:** In the Heston model, the volatility variance $V_t$ is itself an SDE driven by a second Brownian Motion ($B_t^{(2)}$). Notice that the $dV_t$ equation is a modified version of the **Ornstein-Uhlenbeck** process (specifically called a Cox-Ingersoll-Ross process), which forces variance to mean-revert back to a long-term average $\theta$ at a speed of $\kappa$.
- **Correlation:** The two Brownian motions have a specific correlation $\rho$, meaning $dB_t^{(1)} \cdot dB_t^{(2)} = \rho dt$.


## Overview of Numerical Estimation Methods

When an SDE system (like the Heston model) cannot be integrated analytically, quantitative finance relies on numeric solutions.

#### Monte Carlo Methods (The Core of the Project)

- **Concept:** Generate thousands of random paths for $B_t$ using pseudo-random normal numbers, compute the terminal asset values $S_T$, and average the payoffs.
- **Discretization Schemes:** To simulate paths on a computer, continuous SDEs must be broken into discrete steps ($\Delta t$), which introduces approximation errors:
    - _Euler-Maruyama Scheme_ (Stochastic equivalent to Euler's method):
    $$X_{n+1} = X_n + \mu(X_n)\Delta t + \sigma(X_n)\Delta W_n, \quad \Delta W_n \sim \mathcal{N}(0, \Delta t)$$
        
    - _Milstein Scheme_ (Adds a second-order term from Itô's Lemma to improve path accuracy):
        $$X_{n+1} = X_n + \mu(X_n)\Delta t + \sigma(X_n)\Delta W_n + \frac{1}{2}\sigma(X_n)\sigma'(X_n)\left((\Delta W_n)^2 - \Delta t\right)$$
        
- _Project Note:_ In the project, standard Euler discretization of the Heston model can cause the variance $V_t$ to accidentally turn negative in simulations. You will need to analyze adjustments like Full Truncation or Reflection schemes to handle this numerical discretization effect.
    

#### Finite-Difference Methods (FDM)

- **Concept:** Uses the Feynman-Kac theorem to transform the SDE system into a deterministic Partial Differential Equation (PDE). You discretize a spatial-temporal grid and approximate derivatives using central/forward differences to solve for the option price at every grid node.

#### Tree Methods

- **Concept:** Approximates continuous Brownian Motion using discrete binomial or trinomial lattices. At each time increment, the price can only jump up or down with calculated probabilities that match the underlying SDE's drift and volatility.
