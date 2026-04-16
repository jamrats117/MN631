# 🧠 DEEP LEARNING – EXAM CHEAT SHEET
URL: https://youtu.be/alfdI7S6wCY
Title: MIT Introduction to Deep Learning (2025) | 6.S191

---

# 1. 🧩 STRUCTURED SUMMARY (END-TO-END PIPELINE)

## 🔁 Full Training Pipeline (MOST IMPORTANT)
Input X → Forward Pass → Prediction ŷ → Loss L(y, ŷ)  
→ Backpropagation → Gradient ∂L/∂W  
→ Update W → Repeat  

---

## 🧱 Model Structure
- Perceptron: basic unit  
- Neural network = stacked layers  
- Each layer:
  - Linear transform + Nonlinearity  

---

## ⚙️ Core Learning Idea
- Learn mapping:  
  f(X) → y  
- Use data (not rules)  
- Learn hierarchical features automatically  

---

## 📊 Learning Goals
- Minimize loss  
- Generalize to unseen data  
- Avoid:
  - Overfitting  
  - Underfitting  

---

# 2. 📐 KEY FORMULAS (WITH WHEN TO USE)

## 🔹 (1) Perceptron
y = g(WX + b)  
**Use**: Forward pass (always)

---

## 🔹 (2) Multi-layer Network
y = gₙ(Wₙ · gₙ₋₁(... g₁(W₁X)))  
**Use**: Deep models

---

## 🔹 (3) Loss Functions

### ✔ Classification
Cross-Entropy:  
L = -∑ y log(ŷ)  
**Use when**:
- Output = probability  
- Classification task  

---

### ✔ Regression
Mean Squared Error:  
L = (y - ŷ)²  
**Use when**:
- Output = continuous value  

---

## 🔹 (4) Optimization Objective
min_W L(W)  
**Goal**: find best weights  

---

## 🔹 (5) Gradient
∂L/∂W  
**Meaning**:
- Direction of increase  
- Use opposite for update  

---

## 🔹 (6) Gradient Descent
W = W - η ∂L/∂W  

| Symbol | Meaning |
|--------|--------|
| η      | learning rate |

---

## 🔹 (7) Backpropagation (Chain Rule)
∂L/∂W₁ = (∂L/∂y)(∂y/∂z)(∂z/∂W₁)  
**Use**: compute gradients layer-by-layer  

---

## 🔹 (8) Gradient Types

| Method     | Description           | Use Case              |
|------------|----------------------|----------------------|
| GD         | Full dataset         | Accurate but slow    |
| SGD        | 1 sample             | Fast but noisy       |
| Mini-batch | Avg over K samples   | Best practice        |

---

# 3. ⚡ QUICK EXAMPLES (HIGH-YIELD)

## 🎯 Classification Boundary
- Linear model → straight line  
- Nonlinear model → complex boundary  

---

## 🎯 Activation Functions
- Sigmoid → probability (0–1)  
- ReLU → positive outputs only  

---

## 🎯 Training Example
- Input: study hours + lectures  
- Output: pass probability  
- Untrained model → wrong prediction  

---

## 🎯 Optimization
- Start random → move downhill → minimum  

---

## 🎯 Overfitting
- Small data + large model → memorization  
- Train good, test bad  

---

## 🎯 Dropout
- Randomly disable neurons (e.g., 50%)  
- Prevents reliance on single pathway  

---

## 🎯 Early Stopping
- Training loss ↓ continuously  
- Validation loss ↓ then ↑  
- Choose lowest validation loss  

---

# 4. 🧠 EXAM TIPS (CRITICAL)

## 🔥 Tip 1: Identify problem type

| Task          | Output        | Loss           |
|---------------|--------------|----------------|
| Classification| Probability  | Cross-entropy  |
| Regression    | Number       | MSE            |

---

## 🔥 Tip 2: Activation + Loss pairing
- Sigmoid → Binary classification  
- Softmax → Multi-class  
- Linear → Regression  

---

## 🔥 Tip 3: Why deep learning?
- Learns features automatically  
- Handles nonlinear data  
- Scales with data + compute  

---

## 🔥 Tip 4: If model fails
Check:
1. Not trained  
2. Learning rate wrong  
3. Overfitting  
4. Data issue  

---

## 🔥 Tip 5: Gradient intuition
- Gradient = slope  
- Move opposite = minimize  

---

## 🔥 Tip 6: Depth vs Width
- Depth → complexity  
- Width → number of outputs  

---

## 🔥 Tip 7: SGD vs Mini-batch
- SGD → noisy  
- Mini-batch → stable + fast  

---

# 5. ⚠️ COMMON MISTAKES (EXAM TRAPS)

## ❌ Mistake 1: No activation
→ Model becomes linear only  

---

## ❌ Mistake 2: Wrong loss function
- MSE for classification ❌  
- Cross-entropy for regression ❌  

---

## ❌ Mistake 3: Ignoring generalization
- Training accuracy ≠ real performance  

---

## ❌ Mistake 4: Learning rate issues
- Too high → divergence  
- Too low → stuck  

---

## ❌ Mistake 5: Misunderstanding overfitting
Train good + Test bad = overfitting  

---

## ❌ Mistake 6: SGD misunderstanding
- Randomness comes from data sampling  

---

## ❌ Mistake 7: “Deeper is always better”
- More layers = more complexity + harder training  

---

# ✅ FINAL CORE IDEA

Input → Neural Network → Prediction → Loss → Backprop → Gradient → Update → Generalization
