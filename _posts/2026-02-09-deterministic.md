---
title: Deterministic Setting
author: Mgs. M. Luthfi Ramadhan
tags:
  - computing
  - deep learning
  - training
  - experiment
---

# 🔁 Enabling Deterministic Behavior in TensorFlow

This course uses a fixed deterministic setup to ensure:
- Identical results across runs
- Fair and square comparison
- Easier debugging and discussion
- Reproducibility

---

## 1️⃣ Why Determinism Is Needed

Deep learning training can be non-deterministic due to:
- Random initialization
- GPU parallelism
- cuDNN and CUDA auto-optimizations

Without control, the same code may produce different results.

---

## 2️⃣ Required Deterministic Functions

Use the following implementation.

```python
import os
import random
import numpy as np
import tensorflow as tf

SEED = 42 # You can use whatever seed


def set_seeds(seed=SEED):
    os.environ['PYTHONHASHSEED'] = str(seed)
    random.seed(seed)
    tf.random.set_seed(seed)
    np.random.seed(seed)


def set_global_determinism(seed=SEED):
    set_seeds(seed=seed)

    os.environ['TF_DETERMINISTIC_OPS'] = '1'
    os.environ['TF_CUDNN_DETERMINISTIC'] = '1'

    tf.config.threading.set_inter_op_parallelism_threads(1)
    tf.config.threading.set_intra_op_parallelism_threads(1)
    tf.keras.backend.set_floatx('float32')


set_global_determinism(seed=SEED)
```

---

## 3️⃣ Where This Code Must Be Placed
This code must be executed:
At the very top of the Python script
Before:
- Model creation
- Dataset loading
- Any training or evaluation
