---
title: Hyperparameter Search The Cool Way
author: Mgs. M. Luthfi Ramadhan
tags:
  - computing
  - deep learning
  - training
  - experiment
---

# 🔍 Hyperparameter Search Using `script.sh` (Memory-Safe Approach)

Hyperparameter search is done via shell scripts, not Python loops.  
This avoids:
- GPU / RAM memory explosion
- TensorFlow graph accumulation
- Zombie processes after many experiments
- Garbage collecting, you are not a sanitation worker!!!

Each run is a fresh Python process.

---

## 1️⃣ Why NOT Grid Search Inside Python?

❌ Bad practice:
```python
for lr in [1e-3, 1e-4]:
    for bs in [16, 32]:
        model = build_model()
        model.fit(...)
```
Problems:
- GPU memory not fully released (This will literally crash you)
- TensorFlow graph grows
- Hard to recover from crashes
- You have to delete each variable manually and collect the garbage

---

## 2️⃣ Recommended Way
✅ One experiment = one Python process

We use:
- `train.py` → train one configuration
- `script.sh` → launch many runs with different hyperparameter

---

## 3️⃣ train.py: Single-Run Training Script
Your Python script must:
- Accept hyperparameters as arguments
- Train exactly one model
- Exit cleanly

Example: `train.py`
```python
import sys
import tensorflow as tf


batch = int(sys.argv[1])
lr = float(sys.argv[2])
SEED = int(sys.argv[3])


set_global_determinism(seed=SEED)

model = build_model(learning_rate=lr)
model.fit(
    x = x_train,
    y = y_train,
    batch_size = batch,
    epochs = 250
)
```

---

## 4️⃣ script.sh: Grid Search Controller
This script controls the experiment space.

Example: `script.sh`
```bash
#!/bin/bash

lr_pool=("0.001" "0.0005" "0.0001")
batch_pool=("16" "32")
SEED=42

for batch in ${batch_pool[@]}
    do
    for lr in ${lr_pool[@]}
    do
        python train.py "$batch" "$lr" "$SEED"
    done
done

```
`#!/bin/bash` is a must and should be placed on the very first line of the `.sh` script.

---

## 5️⃣ Run your script
⚠️ Make executable (Very Important!!!):

Go to your terminal and run `chmod +x script.sh`

Running your script with the following syntax: `./script.sh`

---

## 6️⃣ This Is Memory-Safe
Each loop:
- Launches a new Python process
- TensorFlow releases GPU memory on exit
- No graph accumulation
- No manual `del model`

---