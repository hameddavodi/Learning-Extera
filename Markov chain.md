
# Markov Chains & Hidden Markov Models — From Zero to Experiments

---

## Part A — Markov Chains (ELI5 but precise)

### Core idea
A Markov chain is a system that moves between states where the **next state depends only on the current state**, not on the full past.

Mathematically:
P(X_{t+1} = j | X_t = i, X_{t-1}, ...) = P(X_{t+1} = j | X_t = i)

---

### States
States are just labels.

Example:
S = {OFF, ON}

---

### Transition matrix
Probabilities of moving between states:

P =
[[0.8, 0.2],
 [0.4, 0.6]]

Rows sum to 1.
Row = current state, Column = next state.

---

### State vector
Where you are *now* (possibly uncertain):

π = [1, 0]   # definitely OFF
π = [0.7, 0.3]  # uncertainty

---

### Time evolution
π_{t+1} = π_t · P

This is just the law of total probability written in matrix form.

---

### Long-run behavior
Repeated multiplication converges to a stationary distribution π*:

π* = π* · P

This tells you how often you are in each state **in the long run**.

---

### Daily-life examples
- Habits (gym / skip)
- Phone keyboard prediction
- Market regimes
- Mood dynamics

Your future distribution is determined by **local transition behavior**.

---

## Part B — Markov Chains at the Lowest Level

### What physically exists
1. A list of states
2. A table of numbers
3. A multiplication rule

Nothing else.

---

### Why matrix multiplication works
π_{next}(j) = Σ_i π(i) · P(i → j)

Just weighted sums.

---

### Minimal Python
```python
import numpy as np

P = np.array([[0.8, 0.2],
              [0.4, 0.6]])

pi = np.array([1.0, 0.0])

for _ in range(10):
    pi = pi @ P
    print(pi)
```

---

## Part C — Hidden Markov Models (HMM)

### What is hidden
You do NOT observe the real state.
You observe noisy signals.

Hidden states:
S = {Healthy, Sick}

Observed signals:
O = {Normal, Fever}

---

### Three probability objects

#### 1. Transition matrix (A)
How states evolve:
A =
[[0.9, 0.1],
 [0.4, 0.6]]

#### 2. Emission matrix (B)
How states generate observations:
B =
[[0.8, 0.2],
 [0.1, 0.9]]

#### 3. Initial distribution
π = [0.7, 0.3]

---

### Generative process
1. Sample next hidden state using A
2. Sample observation using B

Hidden:
H → H → S → S → H

Observed:
N → N → F → F → N

You only see observations.

---

## Part D — Creating Probability Matrices (Real Experiment)

### Method 1 — Assumptions
Used when no data exists.
Rows must sum to 1.

---

### Method 2 — Frequency counting (MLE)

Observed state sequence:
H, H, S, S, S, H, H, S

Count transitions, normalize rows.

Python:
```python
import numpy as np

states = {"H": 0, "S": 1}
seq = ["H","H","S","S","S","H","H","S"]

counts = np.zeros((2,2))

for a, b in zip(seq[:-1], seq[1:]):
    counts[states[a], states[b]] += 1

A = counts / counts.sum(axis=1, keepdims=True)
```

This is maximum likelihood estimation.

---

### Method 3 — Hidden states (Baum–Welch)
When states are unobserved:
1. Guess A, B
2. Compute expected states
3. Update A, B
4. Repeat

This is EM.

---

## Part E — Full HMM Experiment

1. Choose true A and B
2. Generate hidden states
3. Generate observations
4. Delete hidden states
5. Recover A and B

If estimates ≈ true → model works.

---

## What HMMs give you

- Filtering: P(X_t | observations)
- Viterbi path: most likely hidden sequence
- Forecasting future observations

---

## Why this matters (finance intuition)

Hidden: market regime  
Observed: returns, volatility  

HMM = regime-switching with noise.

---

End of file.
