
### Brownian Motion with Drift

- Volatility = standard deviation
- Standard Brownian Motion: $B(0)=0$, mean = 0, std = 1
- General Brownian Motion(with drift): $X(t) = \mu t + \sigma B(t)$, mean $\mu t$, std $\sigma \sqrt t$
- Independent Increments
- $Var[X(t) - X(s)] \propto |t - s|$: variation of increments of the process being proportional to the length of the increment

**Propreties:**
- $E[\Delta X] = \mu \Delta t$
- $Var[\Delta X] = \sigma^2 \Delta t$.
- Exact distribution:  $\Delta X \sim N(\mu\Delta t, \sigma^2 \Delta t)$

**Infinitesimal Analysis**
In very small time steps ($\Delta t \to 0$), **Volatility dominates Drift.**
$$E[(\Delta X)^2] = \sigma^2 \Delta t + (\mu \Delta t)^2 = \sigma^2 \Delta t + o(\Delta t)$$

### Ito Processes

Brownian Motion with Drift: $dX(t) = \mu dt + \sigma dB(t)$ 
- Solution: $X_t = X_0 + \int_0^t \mu ds + \int_0^t \sigma dB_s = X_0 + \mu[t-0] + \sigma[B(t) - B(0)] = X_0 + \mu t + \sigma B(t)$

Ito Process: $dX(t) = \mu(X_t, t)dt + \sigma(X_t, t) dB(t)$ 
 - Extend drift $\mu = \mu(X_t, t)$: we consider a drift $\mu$ that is not a constant, it varies over time and level of the process $X_t$.
- Extend volatility $\sigma = \sigma(X_t, t)$
- Solution:
$$X_t = X_0 + \int_0^t \mu(X_s, s) ds + \int_0^t \sigma(X_s, s) dB_s$$
- $\int_0^t \mu(X_s, s) ds$: standard **Riemann Integral**
- $\int_0^t \sigma(X_s, s) dB_s$: **Itô Integral**. Brownian Motion paths are "jagged" and have infinite total variation, we cannot use standard integration rules.

To define $\int \sigma dB_s$, we have to use the **Adapted Process** rule we just learned. We break the time into tiny steps ($\Delta t$) and define the integral as a sum: $\sum \sigma(X_{t_i}, t_i) [B(t_{i+1}) - B(t_i)]$ 

### Ito Integrals

Consider the Brownian motion process {${B_t , t ≥ 0}$} with:
- Probability model $(Ω, {F_t }, P)$
- $Ω = \{ω\}$ set of all paths ω
- $\{F_t , t ≥ 0\}$: fltration of the process. Events/sets in $F_t$(subsets of $Ω$) that are measurable with respect to {${B_s , s≤t}$} (information set up to time t). 
	- $F_t$ is the field of events/paths wich are fixed/known at time $s ≤ t$
- $P(·)$: probability measure on $Ω$, for all $A \in F_t$, $P(A)$ well defined.
	- which means $P(\Omega)$ is restricted to $s ≤ t$.
- $B_t = B_t(\omega)$: a specific path.

We define the integral $I(f)$ as a random variable. Just like a dice roll, the "answer" to an Itô integral isn't a single number—it’s a **distribution** of possible outcomes depending on which path the stock takes:
	$$I(f )= \int_0^Tf(B_s)dB_s = \int_0^Tf(\omega, s)dB_s$$
Define for each path $\omega$: $$I(f )(ω) = \int_0^Tf (B_s (ω))dB_s (ω)$$

##### Case 1 (The Building Blocks)

1. $f (x) ≡ 1$, we are integrating the number $1$.$$I(f )(ω) = \int_0^TdB_s (ω) = B_T (ω) − B_0(ω)$$$E [I(f )(ω)] = 0$ and $Var [I(f )(ω)] = T$ .
_Intuition:_ If we sum up every tiny change in the Brownian Motion from $0$ to $T$, you just get the total change: $B_T - B_0$.

2.  $f (B_s (ω)) = f (ω, s) = 1(a < s ≤ b)$, we integrate $1$, but only between two specific times, $a$ and $b$.$$I(f )(ω) = \int_0^Tf (ω, s)dB_s (ω) = B_b(ω) − B_a(ω)$$ $E [I(f )(ω)] = 0$ and $Var [I(f )(ω)] = (b-a)$ .
_Result:_ We get the change in the process during that window: $B_b - B_a$.
_Intuition:_ Itô integral behaves like normal calculus when the thing we are integrating (the "integrand") is a simple constant.

##### Case 2 (The "Trading Strategy" Case)
Imagine $f(s)$ is the number of shares we own. We decide at the start of the week exactly how many shares to hold each day (fixed numbers $a_i$). $$f (ω, s) = f_n(ω, s) = \sum_{i=0}^{n-1}a_i 1(t_i < s ≤ t_{i+1})$$where $a_i \in R$, $i = 0, ..., n-1$ and $0 = t_0 < t_1 < ...< t_{n+1} = T$ $$I(f )(ω) = \int_0^Tf (ω, s)dB_s (ω) = \sum_{i=0}^{n−1} a_i[B_{t_{i+1}}(ω) - B_{t_i}(ω)]$$ $E [I(f )(ω)] = 0$ and $Var [I(f )(ω)] = \sum_{i=0}^{n−1} a_i^2[t_{i+1} - t_i]$ .

##### Case 3 (The "Trading Strategy" Case)
Imagine $f(s)$ is the number of shares we own. But the number of shares to hold each day ($a_i$) is now a **random variable** (it can depend on the stock price/path price). $$I(f )(ω) = \sum_{i=0}^{n−1} a_i(ω)[B_{t_{i+1}}(ω) - B_{t_i}(ω)]$$$E [I(f )(ω)] = 0$ and $Var [I(f )(ω)] = \sum_{i=0}^{n−1} E[a_i^2][t_{i+1} - t_i]$

<u>The "No-Cheating" Rule:</u> $a_i$ must be measurable on $\mathcal{F}_{t_i}$, depends on/ be deterministic only on $\{B_s, s≤t_i\}$, which means our decision for the next time step must be based _only_ on the prices we’ve already seen.

**Special Case ($a_i = B_{t_i}$)**: This is the strategy: "I will hold a number of shares equal to the current price." $$I(f )(ω) = \sum_{i=0}^{n−1} B_{t_i}(ω)[B_{t_{i+1}}(ω) - B_{t_i}(ω)]$$ $E [I(f )(ω)] = 0$ and $Var [I(f )(ω)] = \sum_{i=0}^{n−1} E[B_{t_i}^2]E([B_{t_{i+1}} - B_{t_i}]^2) = \sum_{i=0}^{n−1} [t_i][t_{i+1} - t_i] = \frac 1 2 T^2$

<u>The Mean is 0:</u> This is the "Martingale Property." Because we can't see the future, we can't "game" the Brownian Motion. On average, we won't make a profit from the random noise alone.

##### Case 4 (The Big Discovery)
$$f(\omega, s) = \lim_{n \to \infty}f_n(\omega, s) = \lim_{n \to \infty} \sum_{i=0}^{n-1}a_i 1(t_i < s ≤ t_{i+1}) $$ where $t_i = \frac {iT} {n}$, and $a_i(\omega) = B_{t_i}(\omega)$

In Cases 2 and 3, we assumed the "integrand" (the function $f$) was a **step function** (it stays flat for a while, then jumps). But in the real world, a stock price $B_s$ is changing every millisecond.
- To integrate a continuous process like $B_s$, we have to approximate it with tiny steps.
- The "Limit" represents what happens when those steps become **infinitely small**.

Imagine $B_s$ is a stock price and we are a trader:
- **$B_{t_i}$**: This is the stock price at the **start** of a tiny interval of time.
- **$[B_{t_{i+1}} - B_{t_i}]$**: This is the **change** in the stock price over that tiny interval.
If we own $B_{t_i}$ shares of a stock, and the price moves by $(B_{t_{i+1}} - B_{t_i})$, our profit/loss for that tiny slice of time is exactly:
$$\text{Shares} \times \text{Change in Price} = B_{t_i} \cdot [B_{t_{i+1}} - B_{t_i}]$$
The **Summation** ($\sum$) is just we adding up all those tiny profits from the beginning ($0$) to the end ($T$).

In the real world, the stock price doesn't just change at discrete time points ($t_0, t_1, \dots$); it changes continuously. To find the "true" total profit over a continuous path, we have to make the time intervals ($n$) smaller and smaller.
- As $n \to \infty$, the width of each interval goes to zero.
- The sum of these tiny discrete steps becomes the **Integral**.
$$
\begin{align}

I(f )(ω) & = \int_0^T B_s(\omega)dB_s(\omega) \\
 & = \lim_{n \to \infty} \sum_{i=0}^{n−1} B_{t_i}(ω)[B_{t_{i+1}}(ω) - B_{t_i}(ω)] \\ 
& = \lim_{n \to \infty} \sum_{i=0}^{n−1}\frac{1}{2}(B_{t_{i+1}}^2 - B_{t_i}^2) - \sum_{i=0}^{n−1}\frac{1}{2}([B_{t_{i+1}} - B_{t_i}]^2) ]\\
& = \lim_{n \to \infty} [\frac{1}{2} B_{t_n}^2 - \frac{1}{2}\sum_{i=0}^{n−1}([B_{t_{i+1}} - B_{t_i}]^2) ]\\
& = \lim_{n \to \infty} [\frac{1}{2} B_T^2 - \frac{1}{2}\sum_{i=0}^{n−1}([B_{t_{i+1}} - B_{t_i}]^2)] \\ 
& = \frac{1}{2} B_T^2 - \frac{1}{2}\lim_{n \to \infty}\sum_{i=0}^{n−1}(B_{t_{i+1}} - B_{t_i})^2 \\
& = \frac{1}{2} B_T^2 - \frac{1}{2} T


\end{align}
$$

In regular calculus, when we square a tiny change $(\Delta x)^2$, it becomes so small that it disappears in the limit. However in Ito, when we sum up the tiny changes, we get two parts:
- The "Calculus Part": $\frac{1}{2}(B_T^2 - B_0^2)$.
- The "Noise Part": $-\frac{1}{2} \sum (B_{t_{i+1}} - B_{t_i})^2$.
**The Convergence:** we know the sum of squared increments $(\Delta B)^2$ converges to the total time **$T$** (Quadratic Variation). Because Brownian Motion moves so much (Quadratic Variation), "squaring" it creates a deterministic "drift" of $T$. we have to subtract this $T$ to keep the math balanced.


##### Ito Integral: General Case

$$ \begin{align} X_t (ω) & = \int_0^t f (ω, s)dB_s (ω) \\ \\& 0 ≤ t ≤ T \end{align}$$

$f (ω, s)$ : function of $ω$ and time $s$

**Ito Process: $\{X_t (ω), 0 ≤ t ≤ T \}$**
- $E[\int f^2 ds] < \infty$. The expectation of the squared integral must be finite, we can think of this as **Regularization**—we are forcing the function $f$ to stay within a "bounded" space (the $L^2$ space) so the math stays stable.
- $\{X_t\}$ is a Martingale: the **best guess** for the future value of the integral is its **current value**.
$$X_{t+k} = \underbrace{\int_0^t f(s, \omega) dB_s}_{\text{This is } X_t} + \underbrace{\int_t^{t+k} f(s, \omega) dB_s}_{\text{The future increment}}$$
- The Conditional Expectation $E[\dots | X_t]$ $$E[X_{t+k} | X_t] = X_t + E [ f (s,ω)dB_s (ω)] = X_t + 0 = X_t$$
	- "Assume we are standing at time $t$ and we know everything that has happened so far."
	- **For $X_t$:** Since we are standing at time $t$, the value of $X_t$ is already known. It is no longer "random" to us. So, $E[X_t | X_t] = X_t$.
	- **For the increment:** We are left with the expectation of the future piece: $E \left[ \int_t^{t+k} f(s, \omega) dB_s \mid X_t \right]$.
	- **$dB_s$ is a "Pure Surprise":** By the definition of Brownian Motion, the next move ($dB_s$) is independent of everything that happened in the past. Its expected value is $0$.
	- **$f(s, \omega)$ is "Adapted":** Because of the rules we discussed, the trader (us) must decide on the strategy $f$ **before** the surprise $dB_s$ happens.
	- Since we can't predict the surprise, the expected profit for every tiny millisecond between $t$ and $t+k$ is:
$$E[f \times \text{Surprise}] = f \times E[\text{Surprise}] = f \times 0 = 0$$

- Variance and the "Squared-Norm"$$Var[X_{t+k} | X_t] = E[\int_t^{t+k} f^2(s) ds]$$
	- It tells us the **Risk** (Variance) of our strategy between now and time $k$ is simply the total "intensity" of our strategy ($f^2$) over that time.
	- If we hold more shares ($f$ is large), our variance is higher.
	
> [!NOTE]
> Notice that this slide is for a **pure** Itô Integral ($dX = f dB$). If the process had **Drift** ($dX = \mu dt + f dB$), then the expectation would **not** be $X_t$. It would be $X_t + \mu k$.
> 
> Summary in "Student Language": Think of $X_t$ as a **Gambler’s bankroll**. 
> 1. **Measurable:** You place your bets based only on past results.
> 2. **Finite Energy:** You aren't betting infinite amounts of money.
> 3. **Martingale:** The game is "fair"—you can't predict if the next card will be red or black, so your expected wealth in the future is just whatever you have in your pocket right now.
> 4. **Variance:** Your stress level (variance) depends on how much you are betting ($f^2$) and for how long ($ds$).
> 

### Ito Isometry

Provides a bridge between a **random world** (the stochastic integral $dB_s$) and a **measurable world** (the deterministic integral $ds$). 
- The total profit is the integral $I$
$$||I(f)||_{L^2(dP)}^2 = \int_{\Omega} |I(f)(\omega)|^2 dP(\omega) = E\left[ \left( \int_0^T f(s) dB_s \right)^2 \right]$$ $$||f||_{L^2(dP \times dt)}^2 = \int_{\Omega} \int_0^T |f(\omega, s)|^2 ds \, dP(\omega) = E\left[ \int_0^T f(s)^2 ds \right]$$
In  AI studies, we use the $L^2$ norm to measure the "length" of a vector: $||v|| = \sqrt{\sum v_i^2}$. In Stochastic Calculus, the "length" of a random process is its **Standard Deviation**.

The Isometry says:
> The **"Length"** of the total random result ($I(f)$) is exactly the same as the **"Length"** of the strategy ($f$) when measured over time and probability.

To understand why $E[ (\int f dB)^2 ] = E[ \int f^2 dt ]$, let's use a simple two-step example.
Imagine you are trading. You choose to hold $f_1$ shares, then the price moves by $\Delta B_1$. Then you choose to hold $f_2$ shares, and the price moves by $\Delta B_2$.
Your total wealth change is $I = f_1 \Delta B_1 + f_2 \Delta B_2$.
We want to find $E[I^2]$ (the "size" of your total wealth change).
$$\begin{align}

E[I^2] & = E[(f_1 \Delta B_1 + f_2 \Delta B_2)^2] \\ 
&  = E[f_1^2 \Delta B_1^2] + E[f_2^2 \Delta B_2^2] + 2E[f_1 f_2 \Delta B_1 \Delta B_2]

\end{align}
$$ 
- $f_1$ and $f_2$ are **Adapted** (we decided them based on the past).
- $\Delta B_1$ and $\Delta B_2$ are **Independent Surprises**.
    - Because $\Delta B_2$ is a "future surprise," it has no correlation with $f_1, f_2,$ or $\Delta B_1$.
    - The term $2E[\dots \Delta B_2]$ multiplies everything by the "average surprise," which is **zero**. The cross-terms vanish.
So we left with:
$$\begin{align}

E[I^2] & = E[f_1^2 \Delta B_1^2] + E[f_2^2 \Delta B_2^2] \\ \\
& = E[f_1^2] \Delta t + E[f_2^2] \Delta t

\end{align}
$$ In the limit (as $\Delta t \to 0$ and $n \to \infty$), this sum becomes the integral: $$E\left[ \int_0^T f(s)^2 ds \right]$$
By the very definition of **Standard Brownian Motion**, the increment $\Delta B$ (the change between time $s$ and $t$) is defined as: $\Delta B \sim N(0, t - s)$
Since the mean ($\mu$) is $0$, we use the standard variance formula: 
	$Var[\Delta B] = E[\Delta B^2] - (E[\Delta B])^2$
	$t - s = E[\Delta B^2] - (0)^2$
	$E[\Delta B^2] = t - s \quad (\text{which we call } \Delta t)$

> [!NOTE]
> Imagine you are a Quant at a bank and you want to know the risk of a complicated strategy $f$.
> - **Without Isometry:** You would have to simulate 100,000 paths of the Brownian Motion, calculate the integral for each, and then find the variance of those results.
> - **With Isometry:** You don't care about the Brownian Motion! You just look at your strategy $f$, square it, and integrate it over time.
> **The randomness ($dB_s$) has been "canceled out" and replaced by time ($ds$).**
> 
> If you have a very volatile strategy ($f$ is large), your final bankroll ($I$) will have a very wide distribution (large variance). The Isometry tells you the **exact exchange rate** between the two: 1 unit of "strategy energy" ($\int f^2 dt$) creates exactly 1 unit of "financial risk" ($Var[I]$).


### Ito Formula

In standard calculus, the chain rule is simple: if $y = f(x)$, then $dy = f'(x)dx$. However, in quantitative finance, this "cheating" doesn't work because Brownian Motion is too rough. **Itô's Formula** is the correction.

##### Ito’s Formula (One Variable)
$f : R → R$has a continuous second derivative, then
$$df(B_t) = f'(B_t)dB_t + \mathbf{\frac{1}{2}f''(B_t)dt}$$
In Taylor series: $f(x + ∆x) = f (x) + f'(x)∆x + f''(x)(∆x)^2 + r$, we usually ignore $(\Delta x)^2$ because it is too small. But as we discussed, for Brownian Motion, $(\Delta B_t)^2 \approx dt$. So second derivative $f''(B_t)$ **does not disappear.** It survives the limit and becomes a "drift" term.

Solving for $f(B_s )dB_s$
	$F (·) ∈ C^2(R) : F'(·) = f (·)$, $F (0) = 0$
	$F (B_t ) − F (B_0) = \int_0^t f (B_s )dB_s + \frac 1 2 \int_0^t f'(B_s)ds$

**Example 1**: $F (x) = x, f = F' = 1$ and $f'= F'' = 0$
	$\rightarrow \int_0^t dB_s = B_t$
	
**Example 2:** $F(x) = \frac{1}{2}x^2$, $f = F' = x$ and $f' = F'' = 1$
	$\rightarrow \int_0^t B_s dB_s = \frac{1}{2}B_t^2 - \frac{1}{2}t$.
	$\rightarrow$ matches case 4

##### Ito’s Formula: Two Variables (Time and Space)
Option prices decay over **time** ($t$) and change with the **stock price** ($B_t$). When we expand the Taylor series for $f(t, x)$:
$$f (t + ∆t, x + ∆x) = f (t, x) + ∆t\frac{\partial f}{\partial t} (t, x) + ∆x\frac{\partial f}{\partial x} (t, x) + \frac{1}{2} (∆x)^2 \frac{\partial^2 f}{\partial x^2}(t, x) + r$$
- $\frac{\partial f}{\partial t} dt$ comes from the passage of time.
- $\frac{\partial f}{\partial x}∆x = \frac{\partial f}{\partial x} dB_t$ comes from the random move of the stock.    
- $\frac{1}{2} \frac{\partial^2 f}{\partial x^2} (∆x)^2 = \frac{1}{2} \frac{\partial^2 f}{\partial x^2} dt$ comes from the **Quadratic Variation** (the "Itô Tax").

$$f (t, B_t )− f(0, 0) = \int_0^t \left[\frac{\partial f}{\partial t} (s, B_s) + \frac{1}{2} \frac{\partial^2 f}{\partial x^2}(s, B_s)\right] ds + \int_0^t \frac{\partial f}{\partial x} (s, B_s)dB_s$$

In the **Black-Scholes** world, for a price to be "fair," the portfolio must be a **Martingale** (it shouldn't have an expected drift). To achieve this, two opposing forces must cancel each other out perfectly: **Theta** (Time Decay) and **Gamma** (Convexity).
$$\begin{align}

& \left(\frac{\partial f}{\partial t} (s, B_s)+ \frac{1}{2} \frac{\partial^2 f}{\partial x^2}(s, B_s)\right) = 0 \\ \\
& \underbrace{\frac{\partial f}{\partial t}}_{\text{Theta (Loss)}} = \underbrace{-\frac{1}{2} \sigma^2 \frac{\partial^2 f}{\partial x^2}}_{\text{Gamma (Gain)}}

\end{align}
$$
**Force A: Convexity (Gamma / "The Benefit")**
Because an option's payoff is curved (convex), when the stock price moves, you gain more on the "upside" than you lose on the "downside." This curvature is the **$\frac{1}{2} f''(x)$** term in Itô's formula.
- **Intuition:** Volatility is good for option holders. The more the stock "wiggles," the more value you extract from that curvature. This is a **positive** push on the price.
**Force B: Time Decay (Theta / "The Cost")**
You don't get that "convexity benefit" for free. You have to pay for it with time. As every second passes, the option gets closer to its expiration date, and its "uncertainty value" (extrinsic value) drops. This is the **$\frac{\partial f}{\partial t}$** term in Itô's formula.
- **Intuition:** This is a **negative** pull on the price.

**If Gamma > Theta:** You could buy the option, hedge your delta, and you would make "guaranteed" money every time the stock moved, simply because the curvature gain is bigger than the time cost. (This is an **Arbitrage**).

**If Theta > Gamma:** No one would ever buy the option because the cost of "waiting" is higher than any potential benefit from the stock's volatility. The price would have to drop until it reached a balance.

> [!Think of it like an Insurance Policy]
> - The **Convexity** is the "payout" you get if a disaster (a big price move) happens.
> - The **Time Decay** is the "premium" you pay every day to keep the policy active.
> - In a "fair" market, the premium you pay today ($\frac{\partial f}{\partial t}$) must exactly equal the mathematical expectation of the benefit you get from the stock's randomness ($\frac{1}{2} f''$).]


**Example 4: The Exponential Martingale**
This is essentially the "skeleton" of the Black-Scholes stock price model.
<u>The Goal:</u> Prove that $$f(t, B_t) = e^{\sigma B_t - \frac{1}{2}\sigma^2 t}$$ is a Martingale.
$$\frac{\partial f}{\partial t} = -\frac{1}{2} \sigma^2 f, \frac{\partial f}{\partial x} = \sigma f, \frac{\partial^2 f}{\partial x^2} = \sigma^2 f$$
$$ \begin{align}
f (t, B_t ) & = f(0, 0) + \int_0^t \left[\frac{\partial f}{\partial t} (s, B_s) + \frac{1}{2} \frac{\partial^2 f}{\partial x^2}(s, B_s)\right] ds + \int_0^t \frac{\partial f}{\partial x} (s, B_s)dB_s \\ \\
 & = f (0, 0) + \int_0^t 0ds + \int_0^t \sigma e^{\sigma B_t - \frac{1}{2}\sigma^2 t} dB_s \\ \\ 
& = f (0, 0) + \int_0^t \sigma e^{\sigma B_t - \frac{1}{2}\sigma^2 t} dB_s

\end{align}
$$

**Example 5: Brownian Motion with Drift - The Ruin Problem**
This is a classic "Gambler's Ruin" problem, but for a continuous Brownian Motion with drift ($X_t = \mu t + \sigma B_t$).
- <u>The Scenario:</u> You start at 0. You win if you hit $A$ first. You "go ruin" if you hit $-B$ first.
- <u>The Strategy:</u> Instead of doing complex path integrals, we find a function $h(X_t)$ that is a Martingale.
- <u>The Martingale Trick:</u> If $M_t = h(X_t)$ is a Martingale, then its expected value at the end (the stopping time $\tau$) must equal its value at the beginning ($h(0)$). $\rightarrow$ Reminder: because a martingale is _adapted_ to a filtration.
$$E[M_\tau] = P(X_\tau = A) \cdot h(A) + P(X_\tau = -B) \cdot h(-B)$$
	Since we chose $h$ such that $h(A) = 1$ and $h(-B) = 0$, the equation simplifies to:
$$E[M_\tau] = P(X_\tau = A) \cdot 1 + \text{Probability of Ruin} \cdot 0$$
	So, the probability of hitting $A$ is just **$h(0)$: $P(Xτ = A) = E [M_τ ] = M_0 = h(0).$

$$h(X_t ) = h(µt + σB_t ), f (t, x) = h(µt + σx)$$
To make $h(X_t)$ a Martingale, we need the $dt$ terms of the Itô expansion to cancel out. This leads to the Differential Equation:
$$ \begin{align}
f (t, x ) & = f(0, 0) + \int_0^t \left[\frac{\partial f}{\partial t} (s, x) + \frac{1}{2} \frac{\partial^2 f}{\partial x^2}(s, x)\right] ds + \int_0^t \frac{\partial f}{\partial x} (s, x)dx \\ \\
 & = f(0, 0) + \int_0^t \left[µh'(µt + σx) + \frac{1}{2} σ^2h''(µt + σx)\right] ds + \int_0^t \frac{\partial f}{\partial x} (s, x)dx
\end{align}
$$
$$µh'(µt + σx) = -\frac{1}{2} σ^2h''(µt + σx)$$
$$h''(µt + σx) = −(2µ/σ^2)h'(µt + σx)$$
Interpretation: the equation says that the "push" from the drift ($\mu$) must be perfectly balanced by the "spread" from the volatility ($\sigma^2$).


### Geometric Brownian Motion
##### The Standard Process 
$$dX_t = \mu dt + \sigma dB_t$$
For $Y_t = f (t, X_t )$: 
$$dY_t = \frac{\partial f}{\partial t}dt + \frac{\partial f}{\partial x}dX_t + \frac{1}{2}\frac{\partial^2 f}{\partial x^2} \mathbf{dX_t \cdot dX_t}$$
Using the rules of stochastic multiplication:
- $dt \cdot dt = 0$
- $dt \cdot dB_t = 0$
- $dB_t \cdot dB_t = dt$

##### The Markov Case & Itô's Lemma
We focus on the case where the drift and volatility depend on the current state ($X_t$). $$dX_t = \mu(X_t, t)dt + \sigma(X_t, t) dB_t$$ $µ(·, ·)$ and $σ(·, ·)$ are adapted, measurable wrt $\{F_t \}$ 

Given a generalized Wiener process $\{X_t , 0 ≤ t ≤ T\}$, consider the process $Y_t = G (X_t , t)$ where $G$ is a smooth, differentiable function of $X_t$ and $t$. Then: $$dG = \left[\frac{\partial G}{\partial x} \mu (X_t, t) + \frac{\partial G}{\partial t} + \frac{1}{2} \frac{\partial^2 G}{\partial x^2}\sigma^2 (X_t, t)\right] dt +  \frac{\partial G}{\partial x} \sigma(X_t, t)dB_t$$

**Example 2**
$µ(X_t , t) = 0, σ(X_t , t) = 1, G (x, t) = x .$ 
$\frac{\partial G}{\partial x} = 2x, \frac{\partial G}{\partial t}=0, \frac{\partial^2 G}{\partial x^2}=2$
$$⇒ d[X^2] = (2X_t × 0 + 0 + 1 × 2 × 1)dt + 2X_t dX_t = dt + 2X_t dX_t .$$


##### Geometric Brownian Motion
$$dP_t = \mu P_t dt + \sigma P_t dB_t$$The reason that we don't just use $P_t$ directly is because stock prices can't be negative, but Brownian Motion can. To solve this, we look at the **Log-Price**: $G(P_t, t) = \ln(P_t)$.
	$\frac{\partial G}{\partial P} = \frac{1}{P_t}$
	$\frac{\partial G}{\partial t}=0$
	$\frac{\partial^2 G}{\partial P^2} = -\frac{1}{P_t^2}$ $$ \begin{align}
	
	d \ln(P_t) & = \left( \text{Drift} \times \frac{1}{P_t} + \frac{1}{2} \text{Vol}^2 \times -\frac{1}{P_t^2} \right) dt + \left( \text{Vol} \times \frac{1}{P_t} \right) dB_t \\ \\ 
& = \left( \mu P_t \frac{1}{P_t} - \frac{1}{2} \sigma^2 P_t^2 \frac{1}{P_t^2} \right) dt + \left( \sigma P_t \frac{1}{P_t} \right) dB_t \\ \\

& = (\mu - \frac{1}{2}\sigma^2)dt + \sigma dB_t
	\end{align}
	$$ 
Now we integrate from time $t$ to the next time step $t + \Delta t$:
$$\int_t^{t+\Delta t} d \ln(P_s) = \int_t^{t+\Delta t} \left( \mu - \frac{1}{2}\sigma^2 \right) ds + \int_t^{t+\Delta t} \sigma dB_s$$
This gives us:
$$\ln(P_{t+\Delta t}) - \ln(P_t) = \left( \mu - \frac{1}{2}\sigma^2 \right) \Delta t + \sigma (B_{t+\Delta t} - B_t)$$Then we get: 
$$P_{t+\Delta t} = P_t \cdot \exp \left( (\mu - \frac{1}{2}\sigma^2) \Delta t + \sigma (B_{t+\Delta t} - B_t) \right)$$

**Distribution of Returns**
Log-returns are Normally distributed. This is the foundation of **Black-Scholes**: if log-returns are Normal, then the price itself is **Log-Normal**.
Log return on $(t, t + ∆t]$: $$ln(\frac {P_{t+\delta t}} {P_t} ) ∼ N([\mu - \frac{1}{2}\sigma^2](∆t),σ^2∆t)).$$
We can look at the "Log-Return" over a period of time.
- **<u>The Increment:</u>** Because Brownian increments are Normal, the change in the log-price is also Normal.
    - Mean: $(\mu - \frac{1}{2}\sigma^2) \times (\text{Time})$
    - Variance: $\sigma^2 \times (\text{Time})$
- <u>Log-Normal Property:</u> Since $\ln(P_T)$ is Normal, the price $P_T$ itself is **Log-Normal**. This is why stock prices can never be negative (the exponential of any number is positive).

$E[P_T \mid P_t]$ is your **"Best Guess Today"** for what a stock will be worth in the future, given everything you know right now. **$P_t$**: Where the stock is right now.
$$E[P_T | P_t] = P_t e^{(\mu - \frac{1}{2}\sigma^2)(T-t) + \frac{1}{2}\sigma^2(T-t)} = P_t e^{\mu(T-t)}$$

> [!Examples]
>  - Pricing Contracts (The "Fair Value")
> 	 - If $E[P_T \mid P_t] = \$110$, and someone offers to sell you the stock for $\$120$ today, you know it's a bad deal.
>  
>  - Risk Management
> 	 - If you manage a pension fund, you need to know if you'll have enough money to pay retirees in 20 years. Calculating the expectation helps you project whether your current investments are "on track" to meet those future obligations.
> 	 - Because stock prices are **Log-Normal**, the "Average" (Mean) is actually higher than what happens to the "Typical" (Median) investor.
> 	
> 	If you look at 100 people trading this stock: 
> 	- Most people will end up with a bit less than the expectation.
> 	- A few "lucky" people will make a massive amount of money.
> 	- Those few lucky people pull the **Average** ($E[P_T]$) up. 
>
>So, when you calculate $E[P_T \mid P_t]$, you are calculating the **Average**, not the "most likely" outcome.


**Simulating Brownian Motion**

- Time ($T$): 4 years.
- Drift ($\mu$): 0.30 (30% expected return per year).
- Volatility ($\sigma$): 0.40 (40% annualized volatility).

Simulation steps:
1. Discretize Time: Break 4 years into $m$ small steps (e.g., $m=1000$ steps).
2. Generate Noise: At each step, pull a random number from a Normal Distribution: $\epsilon \sim N(0, 1)$.
3. Update the Path: $$P_{t+1} = P_t \cdot \exp \left( (\mu - \frac{1}{2}\sigma^2)\Delta t + \sigma \sqrt{\Delta t} \cdot \epsilon \right)$$
	- $(\mu - \frac{1}{2}\sigma^2)\Delta t$: "Trend". It's the deterministic part that pushes the stock in a direction over time.
	- $\sigma \sqrt{\Delta t} \cdot \epsilon$: "Jiggle".
		- We multiply $\sigma$ by $\sqrt{\Delta t}$ because, as we saw earlier, $E[\Delta B^2] = \Delta t$. To get the "Standard Deviation" (the size of the jump), we take the square root.
		- $\epsilon$ is the "dice roll." Sometimes it's $+2$ (big jump up), sometimes $-1$ (small jump down).
	- In Python, we can start at $P_0 = 100$. We roll the dice to get $P_1$, then we use $P_1$ and roll the dice again to get $P_2$, and so on until we reach the end of the 4 years.
		
4. Log-Returns:
	$$\ln(X_t) - \ln(X_0) \sim N\left(\left[\mu - \frac{\sigma^2}{2}\right]t, \sigma^2 t\right)$$$$R(t) = \frac{\ln(X_t) - \ln(X_0)}{t} \sim N\left(\mu - \frac{\sigma^2}{2}, \frac{\sigma^2}{t}\right)$$
	It is the <u>Annualized Growth Rate</u> of a specific path. If you invested $\$1$ at time $0$ and ended up with $\$X_t$ at time $t$, $R(t)$ tells you what constant interest rate would have given you that same result.
	The Variance ($\sigma^2/t$): the longer you hold a stock ($t$ gets larger), the smaller the variance of your **average annual return** becomes. This is why financial advisors tell you that stocks are "safer" in the long run—the "jiggles" of the Brownian Motion cancel each other out over many years, leaving you closer to the true mean.

Summary: we use the Simulation Steps to create the "jagged" price path. Then we use the Log Return formula $R(t)$ afterward to see how that specific path performed compared to our theory.