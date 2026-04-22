A Brownian motion $W_t$ is a continuous-time stochastic process with independent Gaussian increments: $W_t - W_s∼N(0,t−s)$. We already know Gaussian distributions — the new part is that Brownian motion is a function of time, not a single random variable. So it's a continuous Gaussian process.

✨**Key properties to understand**: 
- Continuity of paths
- Non-differentiability
- Quadratic variation $[W]_t = t$ 
	- This point is what makes stochastic calculus different from classical calculus.

### Difusion Process

A bridge between the **discrete probability** (Markov Chains) and the **continuous-time stochastic processes**. 
- Markov Chain: transition matrix $P_{ij}$
- Brownian Motion: space and time are continuous, the "transition matrix" becomes a **Transition Probability Density Function (pdf)**, $p(y, t | x)$

$Y = B(t)$ given $X = B(s)$, for $t > s$: if you are at position $x$ at time $s$, your position $y$ at a future time $t$ is normally distributed.
- **Mean:** $x$ (The process is a martingale; your best guess for the future is where you are now).
- **Variance:** $\sigma^2(t-s)$. The uncertainty (diffusion) grows linearly with time. This is why volatility in finance is often scaled by $\sqrt{t}$.

**Diffusion Equation**: $$\frac{\partial p}{\partial t} = \frac{1}{2} \sigma^2 \frac{\partial^2 p}{\partial y^2}$$
- $\frac{\partial p}{\partial t}$: how the probability density at a specific point $y$ changes over time
- $\frac{1}{2} \sigma^2 \frac{\partial^2 p}{\partial y^2}$: Laplacian, represents how "concentrated" or "dispersed" a substance is.

	*The Intuition:* Probability "flows" from areas of high concentration to low concentration, just like heat in a metal rod. In finance, this represents the "spreading out" of possible stock prices as time progresses.

**Initial Condition & Conservation**
- Conservation: $\int p \, dy = 1$. This just means the particle (or stock price) has to be _somewhere_. You can't lose probability.
- Initial Condition: At $t=0$, you are exactly at $x$. The probability density is a **Dirac Delta function** (a spike at $x$). As $t > 0$, that spike "diffuses" into the bell curve (Gaussian) you see in the PDF formula.

> [!Quant perspective]
> By solving this equation, we can find the probability of a stock price hitting a certain level. If we add a "drift" term (representing the interest rate), this equation evolves into the **Black-Scholes PDE**.

### Invariance Principle

**Donsker’s Invariance Principle** (also known as the **Functional Central Limit Theorem**). It’s the mathematical bridge that allows us to move from discrete "coin-flip" models to the continuous Brownian Motion used in the Black-Scholes model. As an AI student, we can think of this as the <u>sampling theory</u> for stochastic processes.

Imagine a particle on a line. Every step, it moves either $+1$ or $-1$ (a simple Random Walk). To turn this into a smooth Brownian Motion, we do two things simultaneously:
1. **Shrink the time steps:** Make the steps happen faster and faster (more steps per unit of time).
2. **Shrink the step size:** Make each individual jump smaller so the total variance doesn't explode.
$$B_n(t) = \frac{S_{[nt]}}{\sqrt{n}}$$
- **$S_{[nt]}$:** how many steps we have taken by time $t$. If $n$ is the number of steps per second, then at time $t$, you have taken $[nt]$ steps.
- **$\sqrt{n}$:** we know that the variance of the sum of $k$ i.i.d. variables is $k \times \text{Var}(X)$
	-  if we take $n$ steps in 1 second, the variance would be $n$.
	- To keep the variance "stable" (at 1 per second), we must scale the position by $\frac{1}{\sqrt{n}}$, because $\text{Var}(\frac{S_n}{\sqrt{n}}) = \frac{1}{n} \text{Var}(S_n) = \frac{n}{n} = 1$.
- **Interpretation:** take a random walk, speed up time (so we see many steps quickly), and shrink space (so it doesn’t explode), $B_n(t)$ is a smoothed, rescaled version of a random walk.

**The Variance Result:** $$\text{Var}[B(t)] = t$$$$\text{Var}[B_n(t)] = \text{Var}\left(\frac{S_{[nt]}}{\sqrt{n}}\right) = \frac{1}{n} \text{Var}(S_{[nt]}) = \frac{[nt]}{n}$$
- As $n \to \infty$, the fraction $\frac{[nt]}{n}$ simply becomes $t$.
- This explains why Brownian Motion is often written as $W_t \sim N(0, t)$. The "uncertainty" or "spread" grows linearly with time.

Define $B(t), t ≥ 0$ as $lim_{n→∞}B_n(t)$ $⇒{B(t), t ≥ 0}$ is Standard Brownian Motion Process, **limit is Invariant to distribution of $X_i$**
	Link to ML: if we sum enough independent variables, we get a Gaussian distribution, regardless of whether the original variables were Uniform, Bernoulli, or Poisson.

The **Invariance Principle** (Donsker's Theorem) says the same thing happens for the _entire path_:
- we can build a model where the stock price moves up/down by 1 cent (Bernoulli).
- we can build a model where the stock price moves by a random decimal (Gaussian).
- **As $n \to \infty$, both models converge to the exact same Standard Brownian Motion.**

**Independent Increments**
For any times $t_1 < t_2 < t_3 < t_4$, the change $(B(t_2) - B(t_1))$ is independent of $(B(t_4) - B(t_3))$.
- Finance Meaning: "The market has no memory." Knowing that the stock went up yesterday tells you nothing about whether it will go up today. This is the Weak Form Efficient Market Hypothesis.
- AI/Math Meaning: It is a Markov Process. The future depends only on the current state, not the path taken to get there.

**Variance $\propto |t - s|$**
The variance of the change is proportional to the time elapsed:
$$\text{Var}[B(t) - B(s)] = \sigma^2(t - s)$$
- **The "Square Root of Time" Rule:** If we want to know the standard deviation (volatility), you take the square root: $\text{StdDev} = \sigma \sqrt{\Delta t}$. This is why, if annual volatility is 16%, daily volatility is roughly $16\% / \sqrt{252}$.
	- In finance, **Volatility** ($\sigma$) is defined as the **standard deviation** of returns.
	- Variance → grows like $t$
	- Volatility (std dev) → grows like $\sqrt{t}$
	- If there are 252 trading days in a year:
$$\text{Var}_{\text{annual}} = \sigma_{\text{daily}}^2 \times 252$$$$\sqrt{\text{Var}_{\text{annual}}} = \sqrt{\sigma_{\text{daily}}^2 \times 252}$$$$\sigma_{\text{annual}} = \sigma_{\text{daily}} \times \sqrt{252}$$
> [!Quant perspective]
> In the real market, we don't actually know the exact distribution of stock returns at a micro-second level. But because of this principle, we don't need to! If we assume the steps are small and frequent enough, we are mathematically justified in using **Brownian Motion** as our foundational tool. It simplifies the infinite complexity of market ticks into a single, elegant Gaussian process.
> When we start coding the **Option Pricer**, we will likely use a "Binomial Tree" (the Cox-Ross-Rubinstein model). That tree is exactly what this section describes: a discrete random walk. By understanding the invariance principle, you understand that **a Binomial Tree is just a discretized Brownian Motion.** If we increase the number of steps in our tree to infinity, the price we get from the tree will converge exactly to the Black-Scholes price.
> 

**Python Implementation: The Invariance Principle**
We will use your AI background here. We’ll generate paths using a simple **Bernoulli distribution** (coin flips) and show how, as we increase $n$, it starts looking exactly like the "Gaussian" Brownian Motion used in Black-Scholes.
```python
import numpy as np
import matplotlib.pyplot as plt

def simulate_brownian_motion(T, n):
    """
    T: Total time (e.g., 1 year)
    n: Number of steps
    """
    dt = T / n
    
    # 1. Generate i.i.d steps: Bernoulli flips {-1, 1}
    # This is the 'Xi' from your slide
    steps = np.random.choice([-1, 1], size=n)
    
    # 2. Scale the steps by 1/sqrt(n) as per the Invariance Principle
    # We multiply by sqrt(T) to scale to the total time
    scaled_steps = steps * np.sqrt(dt)
    
    # 3. Compute the path (cumulative sum)
    # This is Bn(t) = S[nt] / sqrt(n)
    path = np.cumsum(scaled_steps)
    
    # Prepend 0 to start at B(0) = 0
    return np.insert(path, 0, 0)

# Parameters
T = 1.0        # 1 Year
steps_list = [10, 100, 1000, 10000]
time_axis = np.linspace(0, T, 10001)

plt.figure(figsize=(12, 6))

for n in steps_list:
    path = simulate_brownian_motion(T, n)
    # Interpolate for plotting comparison
    plt.plot(np.linspace(0, T, n+1), path, label=f'n={n} steps')

plt.title("Convergence of Random Walk to Brownian Motion (Invariance Principle)")
plt.xlabel("Time (t)")
plt.ylabel("B(t)")
plt.legend()
plt.grid(True)
plt.show()
```

#### Questions

1. **If $\text{Var}[B(t)] = \sigma^2 t$, and you know that $B(1) = 10$, what is your expected value for $B(2)$?**
	Brownian motion has independent increments, so $B(2)=B(1)+(B(2)−B(1))$, $B(2)−B(1)$is independent of $B(1)$, $B(2)−B(1)$ has mean $0$ $$
	
	E[B(2) \mid B(1) = 10] = E[B(1) \mid B(1) = 10] + E[B(2) - B(1) \mid B(1) = 10]
						   = 10 + 0 = 10
	$$When we talk about "General" Brownian Motion, there are actually two things we can change: the **Scale (Volatility)** and the **Trend (Drift)**. If they asked $Var[B(t)] = \sigma^2 t$, then it means the **volatility**. Unless they also give us a **$\mu$ (drift)**, we must assume the process is "centered," meaning the expected value doesn't move.
	
| Process Name     | Equation             | Mean $E[Xt​]$ | Variance $Var[Xt​]$ |
| ---------------- | -------------------- | ------------- | ------------------- |
| Standard ($W_t$) | $W_t$                | $0$           | $t$, $\sigma=1$     |
| Scaled ($B_t$)   | $\sigma W_t$         | $0$           | $\sigma^2 t$        |
| General ($X_t$)  | $\mu t + \sigma W_t$ | $\mu t$       | $\sigma^2 t$        |

2. **Difference between standard case and general case**
	if you have a Standard Brownian Motion $W_t$, you can define your General Brownian Motion $B_t$ as: $B_t = \sigma W_t$. Since $\text{Var}[W_t - W_s] = t - s$ (the Standard case), then: $\text{Var}[B_t - B_s] = \text{Var}[\sigma(W_t - W_s)] = \sigma^2 \text{Var}[W_t - W_s] = \sigma^2(t - s)$
	and: $W_t = \frac{B_t - \mu t}{\sigma}$
	The Invariance Principle we studied is the reason we can use the **Standard** version as a building block. It proves that any sum of random "shocks" (with mean 0 and variance 1) converges to that specific $W_t$. Once we have that "Standard" process, we can build any financial model we want by simply stretching it (multiplying by $\sigma$) and tilting it (adding $\mu t$): $X_t = x + \mu t + \sigma W_t$
	
| Feature               | Standard Brownian Motion (Wt​) | **General Brownian Motion (Xt​)**     |
| --------------------- | ------------------------------ | ------------------------------------- |
| Starting Point        | Always starts at 0 ($W_0 = 0$) | Can start anywhere ($X_0 = x$)        |
| Drift ($\mu$)         | $0$ (No trend)                 | Can have a trend (upward or downward) |
| Volatility ($\sigma$) | Fixed at 1                     | Flexible scaling factor               |
| Distribution          | $W_t \sim N(0, t)$             | $X_t \sim N(x + \mu t, \sigma^2 t)$   |
| Equation              | $dW_t$                         | $dX_t = \mu dt + \sigma dW_t$         |

### Refection Principle

	Digital Barrier Options: "Pay $1 if the stock touches $150."
	Lookback Options: "Pay the difference between the maximum price and the final price."

**The "Hitting Time" ($\tau$)**
1. We pick a level $x$ (for example, a stock price of $120).
2. $\tau$ is the first time the Brownian Motion touches that level.

**The Mirror Trick: What is $B^*(t)$**
Imagine the path of a stock. Once it hits level $x$ at time $\tau$, we create a "shadow" path.
- **Before $\tau$:** The reflected path $B^*$ is identical to the original path $B$.
- **After $\tau$:** We take the original path and "flip" it over the horizontal line $y=x$.
    - If the original path went up 2 units above $x$, the reflected path goes down 2 units below $x$.
    - Brownian Motion is symmetric, so the probability models for $B$ and $B^*$ are identical. Once we are at point $x$, the probability of the "future" path going up is exactly the same as it going down. Therefore, the "reflected" set of paths is just as likely to occur as the "original" set of paths.

**The Distribution of the Maximum $M(t)$**
*"What is the probability that the stock touched level $x$ at least once before time $t$?"
$$P[M(t) > x] = 2P[B(t) > x]$$
1. If $B(t) > x$, then the maximum $M(t)$ **must** have been greater than $x$ at some point.
2. But what if $B(t) < x$? Is it possible the maximum was still $> x$? **Yes!** If the path hit $x$ and then wandered back down.
3. Because of the Reflection Principle, for every path that hits $x$ and ends up _above_ it ($B(t) > x$), there is a reflected version that hit $x$ and ended up _below_ it by the same distance.
4. Therefore, the paths that "touched $x$ but ended below it" are exactly as numerous as the paths that "ended above $x$."
$$P(\text{Hit } x) = P(B(t) > x) + P(\text{Hit } x \text{ and } B(t) < x)$$$$P(\text{Hit } x) = P(B(t) > x) + P(B(t) > x) = 2P(B(t) > x)$$
**First Hitting Time**
"The event that the first time we hit $x$ is _before_ time $t$" is exactly the same as "The event that the _maximum_ value reached by time $t$ is at least $x$.
$$\{\omega : \tau(\omega) \leq t\} = \{\omega : M(t) \geq x\}$$
$$P[τ ≤ t] = P[M(t) ≥ x] = 2P[A_{END} (t)] = 2 \times [1 - \Phi(x/\sqrt{t})]$$
- $P[A_{END}(t)]$: "End Event," meaning the event where the Brownian Motion **ends up above $x$** at the final time $t$ ($B(t) > x$).
- $\{B(t)\}$ is a Standard Brownian Motion. By definition: $B(t) \sim N(0, t)$. To use the standard normal table (where the variable is $Z \sim N(0,1)$), we must normalize $B(t)$ by dividing by its standard deviation, which is $\sqrt{t}$: $P[B(t) > x] = P\left( \frac{B(t)}{\sqrt{t}} > \frac{x}{\sqrt{t}} \right) = P\left( Z > \frac{x}{\sqrt{t}} \right)$
- $\Phi(z)$: probability that a variable is **less than** $z$ ($P[Z \leq z]$)

#### Question
If a stock has a volatility $\sigma=1$ and starts at 0, what is the probability that it touches the level $x=2$ at least once within $t=4$ years?
1. Calculate $x/\sqrt{t} = 2 / \sqrt{4} = 2 / 2 = 1$.
2. Find $\Phi(1)$: From a standard normal table, $\Phi(1) \approx 0.8413$.
3. Calculate $1 - \Phi(1)$: $1 - 0.8413 = 0.1587$.
4. Apply the multiplier: $2 \times 0.1587 = \mathbf{0.3174}$ (or **31.74%**).

**The Quant Insight:** Even though the stock ends up above 2 only **~16%** of the time, it _touches_ 2 at least once **~32%** of the time. In other words, half the paths that touch the barrier end up falling back below it.

### Extensions

**Reflected Brownian Motion ($|B(t)|$)**
${B(t), t ≥ 0}$ Standard Brownian Motion, ${R(t), t ≥ 0}$: $R(t) = |B(t)|$, which is an "absolute value" of a path (compared to the symmetry we used in the Reflection Principle)
- it models things that cannot be negative but don't "die" when they hit zero—they just bounce back. Think of **interest rates** in certain economic regimes or **inventory levels**.
- $E[R(t)] = \sqrt{2t/\pi}$, half normal distribution, think of this as the "ReLU" equivalent for stochastic processes.
- If $A(t)$ represents the value of a company's assets, hitting zero means the company is liquidated. The path stops moving because there is no "bouncing back" from bankruptcy.

**Absorbed Brownian Motion (The "Bankruptcy" Model)**
Much common in Credit Risk.
- The process behaves normally until it touches a boundary (usually zero). The moment it hits, it is "trapped" there forever.
- Use it to model the **Default of a Firm**.

**The Brownian Bridge (The "Pinning" Effect)**
Standard Brownian Motion that is **conditioned** to end at a specific value at a specific time (usually $X(1)=0$).
- $X(t) = B(t) - tB(1)$.
    - At $t=0$ (begin), $X(0)=0$.
    - At $t=1$ (end), $X(1) = B(1) - 1(B(1)) = 0$.
- $Cov[X (s), X (t)] = min(t, s) − ts, s, t ∈ [0, 1]$
- $Var[X (t)] = t(1 − t)$. In a normal BM, variance grows forever ($t$). In a bridge, the variance grows until $t=0.5$ and then **shrinks** back to zero as we approach the certain end-point.
- **Quant Use Case:** Used in **Interpolation**. If we know the stock price on Monday and the stock price on Friday, and we want to "guess" the path in between, we use a Brownian Bridge.

**Python Implementation: Simulating the Brownian Bridge**
We will use the formula from your slide: $X(t) = B(t) - tB(1)$. This is essentially "tilting" a standard path so that its end-point is forced back to zero.
```python
def simulate_brownian_bridge(n_paths, n_steps):
    dt = 1.0 / n_steps
    time_axis = np.linspace(0, 1, n_steps + 1)
    
    # 1. Generate Standard Brownian Motion B(t)
    # Increments: dW ~ N(0, sqrt(dt))
    increments = np.random.normal(0, np.sqrt(dt), size=(n_paths, n_steps))
    # Cumulative sum to get paths
    B = np.zeros((n_paths, n_steps + 1))
    B[:, 1:] = np.cumsum(increments, axis=1)
    
    # 2. Extract the final values B(1) for each path
    B_1 = B[:, -1][:, np.newaxis] # Shape (n_paths, 1)
    
    # 3. Construct the Bridge: X(t) = B(t) - t * B(1)
    # time_axis[np.newaxis, :] broadcasts the time vector across all paths
    X = B - time_axis[np.newaxis, :] * B_1
    
    return time_axis, X

# Execution
n_paths = 1000
n_steps = 500
t, bridge_paths = simulate_brownian_bridge(n_paths, n_steps)

# Plotting
plt.figure(figsize=(10, 5))
for i in range(5): # Plot first 5 paths
    plt.plot(t, bridge_paths[i, :], lw=1.5)

plt.title("Brownian Bridge: $X(t) = B(t) - tB(1)$")
plt.xlabel("Time $t$")
plt.ylabel("$X(t)$")
plt.axhline(0, color='black', linestyle='--')
plt.grid(True)
plt.show()
```

#### Question
Calculate the covariance of $X(s)$ and $X(t)$ for $s=0.2$ and $t=0.5$ using the simulation and compare it to the theoretical formula $Cov[X(s), X(t)] = \min(s, t) - st$.
**Theoretical Value:** $Cov(X_{0.2}, X_{0.5}) = \min(0.2, 0.5) - (0.2 \times 0.5) = 0.2 - 0.1 = \mathbf{0.1}$
```python
# Find indices closest to s=0.2 and t=0.5
idx_s = int(0.2 * n_steps)
idx_t = int(0.5 * n_steps)

# Extract the values at those times across all 1000 paths
X_s = bridge_paths[:, idx_s]
X_t = bridge_paths[:, idx_t]

# Calculate covariance matrix
cov_matrix = np.cov(X_s, X_t)
numerical_cov = cov_matrix[0, 1]

print(f"Theoretical Covariance: 0.1")
print(f"Numerical Covariance:   {numerical_cov:.4f}")
```


### Brownian Motion with Drift

Let {$B(t), t≥0$} be a Standard Brownian Motion. Define {$X (t); t≥0$}
$$X(t) = µt + \sigma B(t), t≥0$$

- $µ$ = drift parameter
- $\sigma$ = volatility parameter

This is how we model a stock in the **Black-Scholes** world (though we usually apply this to the log-return). We can think of $X(t) = \mu t + \sigma B(t)$ as a **signal-plus-noise** model:
- **$\mu t$ (The Signal):** A deterministic linear trend. If there were no noise, the stock would just be a straight line.
- **$\sigma B(t)$ (The Noise):** The stochastic part that creates the "wiggles."

**Key Properties**
- Independent Increments
- $Var[X (t) - X (s)] \propto |t - s|$ (Same as for Standard Brownian Motion $µ$ = 0, $\sigma$ = 1.)
- $E[\Delta X] = \mu \Delta t$
- $Var[\Delta X] = \sigma^2 \Delta t$.
- Exact distribution:  $\Delta X \sim N(\mu\Delta t, \sigma^2 \Delta t)$

**Infinitesimal Analysis**
In very small time steps ($\Delta t \to 0$), **Volatility dominates Drift.**
$$E[(\Delta X)^2] = \sigma^2 \Delta t + (\mu \Delta t)^2 = \sigma^2 \Delta t + o(\Delta t)$$
#### Question
If a stock has a drift $\mu = 0.1$ (10% per year) and volatility $\sigma = 0.2$ (20% per year), and the price today is $X(0) = 100$:
1. What is the Expected Value $E[X(1)]$?
2. What is the Variance $Var[X(1)]$?

**Solution:**
1. $E[X(1)] = X(0) + \mu(1) = 100 + 0.1 = \mathbf{100.1}$ (assuming arithmetic drift).
2. $Var[X(1)] = \sigma^2(1) = (0.2)^2 = \mathbf{0.04}$.


### Properties of Paths


**Non-Differentiability**
For a derivative to exist, the ratio $\frac{\Delta B}{\Delta t}$ must converge to a finite number as $\Delta t \to 0$.
As $\Delta t \to 0$, this fraction goes to **infinity**. If we zoom in on a Brownian path, it never looks like a straight line. It stays "jagged" no matter how much you zoom. In AI terms, the "local gradient" is undefined because the noise is too high at every scale.

**Quadratic Variation**
- **Standard Calculus:** If we sum up the squared changes $(\Delta x)^2$ of a smooth function, it goes to **0**.
- ✨**Stochastic Calculus:** If we sum up the squared changes $(\Delta B)^2$ of Brownian Motion, it goes to **T** (the total time).
- $(dB_t)^2 = dt$: while the path is random, its **total accumulated variance** is perfectly predictable.

**Adapted Processes**
"No Time Travel allowed."
- **Filtrations $\mathcal{F}_t$**: The set of all information available up to time $t$.
- **Adapted:** A strategy $Y(t)$ is adapted if we only use information that has already happened to make our decision.

If we are training a Time-Series model, an **Adapted Process** is a model that only uses `x[0:t]` to predict `x[t+1]`. A **Non-Adapted Process** would be like having a "leak" in our training data where the model can see the future labels.

> **Key Rule for Quants:** our trading strategy **must** be adapted. we cannot decide to buy a stock at 10:00 AM based on what the price will be at 4:00 PM.


#### Question
Suppose you define a process $Z(t) = \max_{0 \leq u \leq T} B(u)$ (the maximum value the stock hits over the _entire_ year). Is $Z(t)$ an adapted process at time $t < T$?
_(Hint: Do you know the "Maximum of the whole year" if you are only in February?)

No, $Z(t)$ is not an adapted process. To be adapted to a filtration $\mathcal{F}_t$, a process must be "measurable" based only on the information available at time $t$. In plain English: you must be able to calculate the value of the process using only the history of the stock price up to right now.
If we define $Z(t) = \max_{0 \leq u \leq T} B(u)$:
- $Z(t)$ depends on the maximum value over the **entire year** (from $0$ to $T$).
- If today is February ($t$), you know what the maximum has been _so far_, but you don't know if the stock will rally to a much higher point in October.
- Since the value of $Z(t)$ depends on future events that haven't happened yet, it is **"Forward-Looking."**

**Running Maximum**: $M(t) = \max_{0 \leq u \leq t} B(u)$. Adapted. We only look at history from start to **now**. We can calculate this today.
**Lookback Maximum**: $Z(t) = \max_{0 \leq u \leq T} B(u)$. Not adapted. We are looking at history from start to **the end of the year**.

Imagine we are designing a trading algorithm (an **Adapted Strategy**):
- **Legal Strategy:** "Sell the stock if it drops 5% from its _running maximum_ ($M(t)$)." This is adapted. We can code this.
- **Illegal (Impossible) Strategy:** "Sell the stock today if its price today is within 1% of the _maximum it will ever reach this year_ ($Z(t)$)."
This second strategy is **not adapted**. Unless you have a crystal ball (or "Inside Information"), you cannot mathematically "know" $Z(t)$ until the very last second of the year ($t=T$).