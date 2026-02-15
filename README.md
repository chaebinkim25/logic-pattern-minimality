# Logic–Pattern Minimality Test (LPMT)

An empirical study of the **minimum neural architecture** required to solve structured tasks on synthetic 10×10 binary grids.

---

## 🎯 Goal

This project investigates:

> What is the smallest neural network that can perfectly solve a given task?

We experimentally analyze model complexity under the lens of  
**Minimum Description Length (MDL)**:

L(Model) + L(Data | Model)

We compare Linear models, MLPs, and Convolutional Networks across two regimes:

- **Phase A – Logic & Geometry**
- **Phase B – Pattern Recognition & Translation Invariance**

---

## 🧪 Experimental Setup

**Input:**  
10×10 binary grid (flattened to 100-dimensional vector)

**Loss:**  
CrossEntropy (Softmax)

**Optimizer:**  
SGD or Adam (fixed learning rate)

**Success Criteria:**
- Phase A: 100% test accuracy (no errors allowed)
- Phase B: ≥ 95% test accuracy

---

## 📘 Phase A — Logic & Geometry

Tasks include:

1. Single point localization
2. Selecting leftmost / topmost point
3. Counting (0–2 points, including None)
4. Dual-head min/max coordinate selection
5. Noise robustness stress test

**Hypothesis:**  
Logical and coordinate-based rules can be solved by **Linear models without hidden layers**.

We experimentally verify when non-linearity becomes necessary.

---

## 📗 Phase B — Pattern Recognition

Task:
Classification of 6 structural patterns placed randomly on a 10×10 grid.

Models compared:
- Linear classifier
- 1-hidden MLP
- 1-layer CNN + Global Pooling

**Hypothesis:**  
Translation invariance cannot be efficiently captured by Linear models;  
Convolution provides structural advantage with fewer parameters.

---

## 📊 Research Questions

- When is Linear sufficient?
- When does nonlinearity become necessary?
- When does convolution become structurally optimal?
- Can minimal parameter count predict generalization?

---

## 📂 Repository Structure

```
logic-pattern-minimality/
│
├── models/ # Linear, MLP, CNN
├── data/ # Synthetic grid generators
├── experiments/ # Phase A / Phase B experiments
├── results/ # Accuracy tables and plots
└── analysis/ # MDL estimation tools
```


---

## 🧠 Core Idea

We treat neural networks not as black boxes,
but as **description systems**.

The best model is not the biggest.
It is the **smallest model that fully explains the data**.

---

## 🚀 Future Extensions

- Explicit MDL bit-length computation
- Formal capacity bounds
- Theoretical proof for linear separability in Phase A
- Publication draft (arXiv)

---

## 📜 License

MIT License

---
