# Teaching Retirement Planning with Transferable Examples (Indian Context)

This document is designed to **transfer knowledge to disciples**, not just give answers.
Each section builds intuition → structure → logic → policy realism.

---

## 1️⃣ Excel-Style Inflation-Adjusted Cash-Flow Tables

### Objective
Help learners **see** how money behaves year by year.

---

### Base Assumptions

| Parameter | Value |
|--------|------|
| Retirement age | 55 |
| Horizon | 35 years |
| Monthly expense (today) | ₹50,000 |
| Annual expense (Year 1) | ₹6,00,000 |
| Inflation | 6% |
| Real return | 4% |
| Initial corpus (×40) | ₹2.4 crore |

---

### Cash-Flow Table (Conceptual Excel Layout)

| Year | Age | Opening Corpus (₹) | Annual Expense (₹) | Inflation Adj Expense | Investment Growth | Closing Corpus (₹) |
|----|----|-------------------|--------------------|----------------------|------------------|-------------------|
| 1 | 55 | 2,40,00,000 | 6,00,000 | 6,00,000 | +9,36,000 | 2,43,36,000 |
| 5 | 59 | 2,55,XX,XXX | 6,00,000 | 7,57,000 | +10,XX,XXX | 2,57,XX,XXX |
| 10 | 64 | 2,65,XX,XXX | 6,00,000 | 10,75,000 | +10,XX,XXX | 2,64,XX,XXX |
| 20 | 74 | 2,30,XX,XXX | 6,00,000 | 19,20,000 | +9,XX,XXX | 2,20,XX,XXX |
| 30 | 84 | 1,50,XX,XXX | 6,00,000 | 34,50,000 | +6,XX,XXX | 1,22,XX,XXX |

---

### Teaching Insight

> Expenses rise exponentially  
> Returns rise linearly  
> **This mismatch creates retirement risk**

This table alone explains **why naive formulas fail**.

---

## 2️⃣ Asset Allocation Glide Path (Why It Matters)

### Fixed Allocation (Naive)
60% Equity / 40% Debt forever

### Glide Path (Professional Thinking)

| Age | Equity | Debt | Reason |
|----|------|------|-------|
| 55 | 65% | 35% | Growth needed |
| 60 | 60% | 40% | Volatility control |
| 65 | 50% | 50% | Capital protection |
| 70 | 40% | 60% | Stability |
| 75+ | 30% | 70% | Survival priority |

---

### Why Glide Paths Improve Outcomes

- Reduces **sequence-of-returns risk**
- Locks gains after good markets
- Prevents catastrophic early losses

> Glide paths do NOT maximize wealth  
> They **maximize survival probability**

This is a **core teaching concept**.

---

## 3️⃣ Monte Carlo Logic (Python-Style Pseudocode)

### Goal
Teach *thinking*, not syntax.

---

### Core Idea

> Simulate 1,000 different futures  
> Count how many times money survives

---

### Pseudo-Code (Readable for Non-Coders)

```python
for simulation in range(1000):

    corpus = starting_corpus

    for year in range(35):

        equity_return = random_between(-30%, +25%)
        debt_return   = random_between(4%, 7%)
        inflation     = random_between(5%, 7%)

        portfolio_return = weighted_avg(equity, debt)

        expense = previous_expense * (1 + inflation)

        corpus = corpus * (1 + portfolio_return)
        corpus = corpus - expense

        if corpus <= 0:
            mark_simulation_as_failed()
            break

calculate_survival_probability()
