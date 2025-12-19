# Monte Carlo Simulation for Retirement Planning  
## A Rigorous Mathematical Treatment (1000-Scenario Framework)

---

## 1. Formal Problem Definition

We consider a **retirement decumulation problem** under uncertainty.

Let:

- \( T \) = retirement horizon in years (e.g., \( T = 35 \))
- \( W_t \) = withdrawal (expense) at year \( t \)
- \( C_t \) = portfolio value at start of year \( t \)
- \( R_t \) = random portfolio return in year \( t \)
- \( \pi_t \) = inflation rate in year \( t \)

The system evolves as:

\[
C_{t+1} = (C_t - W_t)(1 + R_t)
\]

with the constraint:

\[
C_t \ge 0 \quad \forall t \in [0,T]
\]

**Ruin occurs** if \( \exists t \) such that \( C_t \le 0 \).

---

## 2. Stochastic Modeling of Returns and Inflation

### 2.1 Asset Returns

Let the portfolio consist of equity and debt.

\[
R_t = w_e R^e_t + w_d R^d_t
\]

where:
- \( w_e + w_d = 1 \)

#### Equity Returns

Empirically modeled as:

\[
R^e_t \sim \mathcal{N}(\mu_e, \sigma_e^2)
\]

Typical Indian long-term parameters (nominal):
- \( \mu_e \approx 0.12 \)
- \( \sigma_e \approx 0.20 \)

#### Debt Returns

\[
R^d_t \sim \mathcal{N}(\mu_d, \sigma_d^2)
\]

- \( \mu_d \approx 0.06 \)
- \( \sigma_d \approx 0.03 \)

---

### 2.2 Inflation Process

Inflation is modeled independently as:

\[
\pi_t \sim \mathcal{N}(\mu_\pi, \sigma_\pi^2)
\]

Indian parameters:
- \( \mu_\pi \approx 0.06 \)
- \( \sigma_\pi \approx 0.015 \)

---

## 3. Inflation-Indexed Withdrawals

Initial annual withdrawal:

\[
W_0 = E_0
\]

where \( E_0 \) is current annual expense.

Inflation-indexed withdrawal:

\[
W_t = E_0 \prod_{i=1}^{t} (1 + \pi_i)
\]

This creates **exponentially increasing withdrawals**, a key destabilizing force.

---

## 4. Monte Carlo Simulation Framework

We simulate \( N = 1000 \) independent paths.

For each simulation \( s \in \{1,2,\dots,N\} \):

1. Sample:
   - \( \{R_t^{(s)}\}_{t=1}^{T} \)
   - \( \{\pi_t^{(s)}\}_{t=1}^{T} \)
2. Compute withdrawals \( \{W_t^{(s)}\} \)
3. Evolve wealth recursively
4. Record survival or ruin

---

## 5. Recursive Wealth Dynamics (Core Equation)

For scenario \( s \):

\[
C_{t+1}^{(s)} = \left(C_t^{(s)} - W_t^{(s)}\right)
\left(1 + w_e R^e_t + w_d R^d_t\right)
\]

with initial condition:

\[
C_0^{(s)} = C_0
\]

---

## 6. Definition of Survival Indicator

Define indicator variable:

\[
I^{(s)} =
\begin{cases}
1, & \text{if } C_t^{(s)} > 0 \quad \forall t \le T \\
0, & \text{otherwise}
\end{cases}
\]

---

## 7. Estimated Probability of Retirement Success

The Monte Carlo estimator of survival probability:

\[
\hat{P}_{\text{survival}} =
\frac{1}{N} \sum_{s=1}^{N} I^{(s)}
\]

By the **Law of Large Numbers**:

\[
\hat{P}_{\text{survival}} \xrightarrow[]{a.s.} P_{\text{true}}
\quad \text{as } N \to \infty
\]

---

## 8. Statistical Properties of the Estimator

Since \( I^{(s)} \sim \text{Bernoulli}(p) \),

\[
\text{Var}(\hat{P}) = \frac{p(1-p)}{N}
\]

For \( N = 1000 \), worst-case variance:

\[
\text{SE} \le \sqrt{\frac{0.25}{1000}} \approx 1.6\%
\]

Thus:
> **1000 simulations give statistically meaningful confidence**

---

## 9. Sequence of Returns Risk (Formal Explanation)

Let cumulative return:

\[
\prod_{t=1}^{T} (1 + R_t)
\]

Even if:

\[
\mathbb{E}[R_t] = \text{constant}
\]

the distribution of \( C_T \) depends on **ordering** because withdrawals occur *before compounding*.

This violates commutativity:

\[
(W \circ R_1 \circ R_2) \neq (W \circ R_2 \circ R_1)
\]

Monte Carlo captures this **path dependence**.

---

## 10. Failure Probability as a Stopping Time

Define ruin time:

\[
\tau = \inf \{ t : C_t \le 0 \}
\]

The retirement problem becomes estimating:

\[
P(\tau > T)
\]

This is a **first-passage time problem**, analytically intractable → Monte Carlo required.

---

## 11. Why Closed-Form Solutions Do Not Exist

The system involves:
- Stochastic multiplicative growth
- Deterministic but exponential withdrawals
- Inequality constraints \( C_t \ge 0 \)

This forms a **non-linear stochastic difference equation** with absorbing boundary.

Hence:
> Monte Carlo is not a choice — it is a necessity.

---

## 12. Interpretation of Results (Example)

| Corpus Rule | Estimated \( \hat{P}_{\text{survival}} \) |
|-----------|--------------------------------|
| ×30 | 0.55 |
| ×40 | 0.78 |
| ×50 | 0.92 |

These are **probabilities**, not guarantees.

---

## 13. Mathematical Limitations

Monte Carlo accuracy depends on:
- Correct parameter estimation
- Independence assumptions
- Stationarity of returns

But it is **asymptotically correct** given assumptions.

---

## 14. Canonical References (Authoritative Sources)

### Academic Papers
1. Bengen, W. (1994). *Determining Withdrawal Rates Using Historical Data*. Journal of Financial Planning.
2. Pfau, W. (2011). *Safe Withdrawal Rates: A Guide for Retirees*. Journal of Financial Planning.
3. Milevsky, M. (2006). *The Calculus of Retirement Income*.

---

### Books (Mathematical Treatment)
4. **Moshe Milevsky** – *Are You a Stock or a Bond?*
5. **Zvi Bodie, Alex Kane, Alan Marcus** – *Investments*
6. **Paul Glasserman** – *Monte Carlo Methods in Financial Engineering*

---

### Actuarial & Institutional Sources
7. Society of Actuaries – Retirement income modeling papers
8. Vanguard Research – Monte Carlo retirement sustainability
9. Morningstar Methodology Papers

---

## 15. Final Mathematical Insight

> Retirement planning is a **stochastic control problem under uncertainty**.

> Monte Carlo simulation approximates the **distribution of outcomes**,  
> not a single misleading expectation.

This is why **every serious retirement model in the world uses Monte Carlo**.
