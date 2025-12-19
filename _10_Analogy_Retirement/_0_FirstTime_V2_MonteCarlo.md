# Monte Carlo Simulation for Retirement Planning  
## (1000-Scenario Framework – Indian Context)

This document explains **Monte Carlo simulation for retirement planning** in a way that:
- Transfers *understanding*, not just formulas
- Avoids heavy mathematics
- Matches Indian inflation, markets, and retirement realities
- Is suitable for teaching disciples and practitioners

---

## 1️⃣ What Monte Carlo Simulation Really Means (Plain Language)

Monte Carlo simulation answers **one fundamental question**:

> “If the future can unfold in many unpredictable ways,  
> how often will my retirement plan survive?”

Instead of assuming:
- One inflation rate
- One return number
- One smooth future  

Monte Carlo creates **hundreds or thousands of alternate futures**.

Each future is called a **scenario**.

---

## 2️⃣ Why Retirement Planning NEEDS Monte Carlo

### Deterministic Planning (Wrong Assumption)

Traditional planning assumes:
- Fixed 8% return
- Fixed 6% inflation
- Straight-line growth

This future **never exists in reality**.

---

### Reality (What Actually Happens)

- Markets jump and crash
- Inflation spikes unexpectedly
- Bad years cluster together
- Medical expenses rise late in life

Monte Carlo **embraces randomness instead of ignoring it**.

---

## 3️⃣ Core Idea: 1000 Different Futures

Monte Carlo does NOT predict the future.

It asks:
> “Out of 1000 possible futures,  
> how many times does my money last 35 years?”

This gives a **probability of survival**, not a false guarantee.

---

## 4️⃣ Structure of One Monte Carlo Scenario

Each scenario simulates **one complete retirement lifetime**.

### One Scenario Includes:

| Component | What Changes Randomly |
|--------|----------------------|
| Equity return | High, low, or negative |
| Debt return | Stable but variable |
| Inflation | Normal or spike years |
| Expenses | Increase with inflation |
| Sequence | Order of returns |

---

## 5️⃣ Indian-Specific Assumptions (Used in Simulation)

### Returns (Nominal)

| Asset | Range Used |
|-----|-----------|
| Equity | –30% to +25% |
| Debt | +4% to +7% |

### Inflation

| Type | Range |
|----|------|
| General | 5% – 7% |
| Medical (late years) | 8% – 10% |

---

## 6️⃣ Pseudo Monte Carlo Logic (Readable, Not Mathematical)

### Conceptual Pseudocode

```python
successful_runs = 0

for scenario in range(1000):

    corpus = starting_corpus
    expense = starting_expense

    for year in range(35):

        equity_return = random_value(-0.30, +0.25)
        debt_return   = random_value(0.04, 0.07)
        inflation     = random_value(0.05, 0.07)

        portfolio_return = (
            equity_weight * equity_return +
            debt_weight * debt_return
        )

        expense = expense * (1 + inflation)
        corpus  = corpus * (1 + portfolio_return)
        corpus  = corpus - expense

        if corpus <= 0:
            break

    if corpus > 0:
        successful_runs += 1

survival_probability = successful_runs / 1000
```




### 7️⃣ The Most Important Insight: Sequence of Returns Risk
Two Retirees – Same Average Return
Retiree	Market Order	Outcome
A	Bad years first	❌ Ruin
B	Good years first	✅ Survives

Even if:

Average return = same

Total return = same

Order matters more than average.

Monte Carlo captures this. Simple formulas do not.

### 8️⃣ How to Interpret Monte Carlo Results
Example Output
Corpus Rule	Survival % (35 yrs)
×30	58%
×40	78%
×50	92%
Teaching Interpretation

58% → Flip of a coin (dangerous)

78% → Acceptable risk

92% → High confidence

Monte Carlo does not say “this will happen”
It says “this happens often or rarely”

### 9️⃣ Why 1000 Simulations?
Simulations	Reliability
10	Meaningless
100	Noisy
1000	Stable intuition
10,000	Professional-grade

1000 is:

Computationally light

Intuitively reliable

Widely used in practice

### 10️⃣ What Monte Carlo Still Cannot Do

Monte Carlo is powerful, but not divine.

It cannot:

Predict wars

Predict policy shocks

Predict personal health events

It models uncertainty, not certainty.

### 11️⃣ Teaching Wisdom (For Disciples)
Monte Carlo Teaches 5 Deep Lessons

The future is not single-valued

Averages are misleading

Early losses are dangerous

Safety comes from buffers, not forecasts

Retirement planning is probabilistic

### 12️⃣ Why Professionals Trust Monte Carlo

Monte Carlo is used by:

Pension funds

Insurance actuaries

Endowment managers

Retirement planners globally

Because:

It respects uncertainty instead of denying it

### 13️⃣ Authoritative References (Foundational Sources)
Books

William F. Sharpe – Investors and Markets

Moshe Milevsky – Are You a Stock or a Bond?

Wade Pfau – Retirement Planning Guidebook

Zvi Bodie – Risk Less and Prosper

Academic & Professional Work

Bengen, W. (1994): Determining Withdrawal Rates Using Historical Data

Pfau, W. (2010–2018): Safe Withdrawal Rates & Monte Carlo retirement research

Society of Actuaries – Retirement sustainability modeling papers

Industry Usage

Vanguard retirement research

Morningstar retirement simulations

Fidelity & BlackRock retirement planning tools

(All rely on Monte Carlo–style scenario modeling.)

### 14️⃣ Final Teaching Statement

A retirement plan that survives only one future
is not a plan.

A retirement plan that survives most futures
is wisdom.

Monte Carlo simulation transforms retirement planning
from hope-based arithmetic into risk-aware strategy.





---

If you want next:
- I can convert this into a **real Python notebook**
- Or a **step-by-step Excel Monte Carlo**
- Or a **one-page guru-style teaching diagram**
- Or adapt it to **Indian tax-adjusted post-tax Monte Carlo**

Just tell me how you want this knowledge to live on.
