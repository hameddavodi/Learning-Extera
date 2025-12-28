Think of a Markov chain as a **game where the future only cares about where you are now**, not how you got there.

---

### ELI5 idea

Imagine the weather:

* Today can be **Sunny**, **Cloudy**, or **Rainy**
* Tomorrow’s weather depends **only on today**, not on the whole past week

If today is Sunny:

* Tomorrow is Sunny with probability 0.6
* Cloudy with probability 0.3
* Rainy with probability 0.1

That’s a Markov chain.

---

### The core rule (the “Markov property”)

Mathematically:

[
P(X_{t+1} = j \mid X_t = i, X_{t-1}, \dots, X_0)
================================================

P(X_{t+1} = j \mid X_t = i)
]

Translation:
**The next state depends only on the current state.**

---

### States

A Markov chain has:

* A **set of states**:
  [
  S = {s_1, s_2, \dots, s_n}
  ]

Example (mood):
[
S = {\text{Happy}, \text{Neutral}, \text{Sad}}
]

---

### Transition probabilities

You describe how likely it is to move from one state to another using a **transition matrix**:

[
P =
\begin{bmatrix}
0.7 & 0.2 & 0.1 \
0.3 & 0.4 & 0.3 \
0.2 & 0.3 & 0.5
\end{bmatrix}
]

Row = today
Column = tomorrow

Each row sums to 1:
[
\sum_j P_{ij} = 1
]

---

### State vector (where you are now)

Your current situation is a probability vector:

[
\pi_t = [0.5,\ 0.3,\ 0.2]
]

Meaning:

* 50% Happy
* 30% Neutral
* 20% Sad

Tomorrow:

[
\pi_{t+1} = \pi_t \cdot P
]

This is just matrix multiplication.

---

### Long-run behavior (steady state)

If you keep applying the transitions:

[
\pi_{t+k} = \pi_t P^k
]

Often, it converges to a **stationary distribution**:

[
\pi^* = \pi^* P
]

This tells you:

> “In the long run, how often am I in each state?”

---

### How you use Markov chains in daily life

#### 1. Predicting habits

States:

* Gym
* Home
* Skip

You estimate transition probabilities from your behavior.
You can answer:

* “If I skip today, how likely am I to skip tomorrow?”
* “What’s my long-run gym attendance?”

---

#### 2. Text prediction (your phone keyboard)

States = words
Transition:
[
P(\text{next word} \mid \text{current word})
]

Example:
[
P(\text{"coffee"} \mid \text{"drink"}) = 0.4
]

This is literally a Markov chain.

---

#### 3. Finance (very close to your background)

Market regimes:
[
S = {\text{Bull}, \text{Sideways}, \text{Bear}}
]

You model:
[
P(\text{Bear}_{t+1} \mid \text{Bull}_t)
]

Used in:

* Regime-switching models
* Credit ratings (AAA → AA → A …)
* Customer churn modeling

---

#### 4. Decision making under uncertainty

States = situations in life
You ask:

* “If I’m here now, where do I most likely end up next?”
* “What happens if I repeat this behavior?”

---

### One-sentence intuition

A Markov chain is **a probabilistic “if I’m here now, what happens next?” machine**, where the past doesn’t matter once you know the present.
Let’s go **as low-level as possible**, almost like opening the engine and touching the gears.

---

## Step 0 — What exists, physically?

A Markov chain is just **three concrete things**:

1. A **list of states**
2. A **table of numbers**
3. A **rule for updating numbers**

Nothing more.

---

## Step 1 — States are just labels

Example: a light switch

[
S = {0, 1}
]

* `0` = OFF
* `1` = ON

These are **not numbers for math**, just names.

---

## Step 2 — Transitions are coins you flip

From each state, you flip a biased coin to decide the next state.

Example:

* If OFF:

  * stay OFF with probability 0.8
  * go ON with probability 0.2

* If ON:

  * stay ON with probability 0.6
  * go OFF with probability 0.4

Write this as a matrix:

[
P =
\begin{bmatrix}
0.8 & 0.2 \
0.4 & 0.6
\end{bmatrix}
]

Read it **row by row**:

* Row 0 → what happens if you are OFF
* Row 1 → what happens if you are ON

---

## Step 3 — What “state” really means (important)

There are **two ways** to describe where you are.

### A) You are definitely somewhere

“I am OFF right now”

This is written as:

[
\pi = [1, 0]
]

### B) You are uncertain

“I’m OFF with 70%, ON with 30%”

[
\pi = [0.7, 0.3]
]

This vector is **not philosophy** — it’s bookkeeping.

---

## Step 4 — Time moves by multiplication

This is the core mechanic.

Tomorrow’s belief:

[
\pi_{t+1} = \pi_t \cdot P
]

Let’s compute it **by hand**.

Start OFF:

[
\pi_0 = [1, 0]
]

After one step:

[
\pi_1 =
[1, 0]
\begin{bmatrix}
0.8 & 0.2 \
0.4 & 0.6
\end{bmatrix}
=============

[0.8, 0.2]
]

Interpretation:

* 80% OFF
* 20% ON

---

## Step 5 — Why matrix multiplication works

Take the first component:

[
\pi_{1,\text{OFF}} =
(1 \times 0.8) + (0 \times 0.4)
]

Translation:

* “Probability I was OFF” × “probability OFF→OFF”
* plus
* “Probability I was ON” × “probability ON→OFF”

This is **law of total probability**, nothing fancy.

---

## Step 6 — Repeating behavior = powers of the matrix

After 2 steps:

[
\pi_2 = \pi_1 P
]

After 10 steps:

[
\pi_{10} = \pi_0 P^{10}
]

This models:

> “What happens if I keep behaving the same way?”

---

## Step 7 — Steady state (the end of the movie)

Eventually:

[
\pi_{t+1} \approx \pi_t
]

So:

[
\pi^* = \pi^* P
]

Solve this with:

* one linear equation
* plus (\sum \pi_i = 1)

This tells you:

> “In the long run, how often am I ON vs OFF?”

This is **not prediction**, it’s equilibrium.

---

## Step 8 — Daily-life example (habits)

States:
[
S = {\text{Work}, \text{Scroll}, \text{Sleep}}
]

You estimate transitions from observation:

* After Work:

  * Scroll: 0.6
  * Sleep: 0.3
  * Work: 0.1

* After Scroll:

  * Scroll: 0.7
  * Sleep: 0.2
  * Work: 0.1

This answers:

* “Why do I get stuck scrolling?”
* “What happens if I change one transition?”

Small probability changes → big long-run changes.

---

## Step 9 — Why this matters deeply

Markov chains teach one brutal truth:

> **Your future distribution is determined by your local behavior.**

Not goals.
Not intentions.
Just transition probabilities.

---

## Step 10 — Minimal Python (nothing hidden)

```python
import numpy as np

P = np.array([[0.8, 0.2],
              [0.4, 0.6]])

pi = np.array([1.0, 0.0])

for _ in range(10):
    pi = pi @ P
    print(pi)
```

This is **exactly** the math above. No tricks.

### Part 1 — Hidden Markov Models (HMM), *from zero atoms*

A **Hidden Markov Model** is just a Markov chain **you cannot directly see**.

You have:

* **Hidden states** (real situation)
* **Observed signals** (what you can measure)

You never see the states.
You only see noisy clues.

---

## Step 1 — What is “hidden”, concretely?

Example: you are sick or healthy.

Hidden states (real, unobservable):
[
S = {\text{Healthy}, \text{Sick}}
]

Observations (what you see):
[
O = {\text{Normal}, \text{Fever}}
]

You don’t see *Healthy/Sick*.
You see *Normal/Fever*.

---

## Step 2 — HMM = 3 tables (nothing else)

An HMM has **exactly three probability objects**.

---

### 1️⃣ Transition matrix (same as Markov chain)

“How states evolve”

[
A =
\begin{bmatrix}
P(H \to H) & P(H \to S) \
P(S \to H) & P(S \to S)
\end{bmatrix}
=============

\begin{bmatrix}
0.9 & 0.1 \
0.4 & 0.6
\end{bmatrix}
]

---

### 2️⃣ Emission matrix (new thing)

“How states produce observations”

[
B =
\begin{bmatrix}
P(\text{Normal} \mid H) & P(\text{Fever} \mid H) \
P(\text{Normal} \mid S) & P(\text{Fever} \mid S)
\end{bmatrix}
=============

\begin{bmatrix}
0.8 & 0.2 \
0.1 & 0.9
\end{bmatrix}
]

This is **sensor noise**.

---

### 3️⃣ Initial state distribution

[
\pi = [0.7, 0.3]
]

“I believe I start Healthy with 70%”

---

## Step 3 — What actually happens in reality

At each time step:

1. Hidden state changes (using **A**)
2. Observation is generated (using **B**)

Example:

```
Hidden:   H → H → S → S → H
Observed: N → N → F → F → N
```

You only see:

```
N, N, F, F, N
```

Your job:

> infer the hidden states and probabilities

---

## Step 4 — Why HMM is NOT magic

An HMM is just:

* counting
* multiplying probabilities
* normalizing

No AI vibes.

---

# Part 2 — “Default probabilities”: how do we *create* the matrices?

There are **only 3 legitimate ways**.

---

## Method A — Pure assumptions (baseline model)

Used when:

* no data
* you want a sandbox

Rules:

* rows must sum to 1
* transitions should be *plausible*

Example baseline:

```python
A = np.array([[0.8, 0.2],
              [0.3, 0.7]])

B = np.array([[0.9, 0.1],
              [0.2, 0.8]])
```

This is **not learned**, just hypothesized.

---

## Method B — Frequency counting (real experiment)

This is the **most important one**.

---

### Step 1 — Run an experiment

Let’s say you **actually observe states** (for learning).

Example data:

```
H, H, S, S, S, H, H, S
```

---

### Step 2 — Count transitions

Count how many times each transition happens:

| From \ To | H | S |
| --------- | - | - |
| H         | 2 | 2 |
| S         | 1 | 2 |

---

### Step 3 — Normalize rows

[
P(H \to H) = \frac{2}{4} = 0.5
]

[
P(H \to S) = \frac{2}{4} = 0.5
]

This is **Maximum Likelihood Estimation**.

---

### Python (explicit, no shortcuts)

```python
import numpy as np

states = {"H": 0, "S": 1}
seq = ["H","H","S","S","S","H","H","S"]

counts = np.zeros((2,2))

for a, b in zip(seq[:-1], seq[1:]):
    counts[states[a], states[b]] += 1

A = counts / counts.sum(axis=1, keepdims=True)
```

That’s it. No ML library needed.

---

## Method C — Learn from observations only (true HMM learning)

This is the **hard case**.

You only see:

```
N, N, F, F, N
```

You don’t know the states.

This uses **EM / Baum–Welch**.

Conceptually:

1. Guess A, B
2. Compute expected hidden states
3. Update A, B
4. Repeat until stable

This is iterative probability bookkeeping.

---

# Part 3 — Full experiment: simulate → hide → recover

This is the clean mental model.

---

## Step 1 — Simulate the real world

You **choose true A and B**.

```python
A_true = np.array([[0.9, 0.1],
                   [0.3, 0.7]])

B_true = np.array([[0.8, 0.2],
                   [0.1, 0.9]])
```

---

## Step 2 — Generate hidden states

This is just biased coin flips.

---

## Step 3 — Generate observations

Another biased coin flip, conditional on state.

---

## Step 4 — Throw away hidden states

Now you are blind.

---

## Step 5 — Recover probabilities

Use Baum–Welch to estimate A and B.

If estimates ≈ true → model works.

---

## Step 6 — What HMM gives you in practice

1. **Filtering**
   [
   P(X_t \mid O_1, \dots, O_t)
   ]

2. **Most likely path (Viterbi)**
   “What actually happened?”

3. **Forecasting**
   [
   P(O_{t+1} \mid O_{1:t})
   ]

---

## Why this matters for you (very concrete)

Given your background:

* **Market regimes** (hidden)
* **Prices/returns** (observed)

HMM = regime-switching model with noise.

This is why HMMs appear in:

* finance
* speech recognition
* behavior modeling
* anomaly detection

---



