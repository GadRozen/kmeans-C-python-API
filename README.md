Project guidelines by Tel Aviv University Software Project Course:
[Final_Project (7).pdf](https://github.com/user-attachments/files/22935159/Final_Project.7.pdf)

# 🧮 Symmetric Non-Negative Matrix Factorization (SymNMF) & K-Means Clustering

### The project was done in collaboration with **Gad Rozen** and **Hila Etzioni**

---

## 📘 Overview

This project implements and compares two clustering algorithms — **Symmetric Non-Negative Matrix Factorization (SymNMF)** and **K-Means** — using both **C** and **Python**.  
It demonstrates algorithmic design, efficient computation, and integration between low-level and high-level languages through a custom **C–Python extension module**.

The implementation calculates **silhouette scores** to evaluate and compare the quality of clustering results for both algorithms.

---

## ⚙️ Technologies & Tools

| Category | Tools / Technologies |
|-----------|----------------------|
| **Programming Languages** | Python 3, C (ANSI C 90) |
| **Python Libraries** | `numpy`, `scikit-learn`, `setuptools` |
| **Extension Binding** | Custom CPython C Extension (`symnmfmodule.c`) |
| **Build System** | `Makefile`, `setup.py` |
| **Version Control** | Git & GitHub |
| **Platform** | Linux / macOS compatible (CLI-based) |

---

## 🧩 Architecture

The project is composed of three main layers:

1. **Core Algorithms (C layer)**  
   Implements efficient numerical operations for:
   - **A** – Similarity matrix using Gaussian kernel  
   - **D** – Diagonal degree matrix  
   - **W** – Normalized similarity matrix (W = D^-1/2 A D^-1/2)  
   - **SymNMF** – Multiplicative updates of H until convergence (β=0.5, ε=1e−4)

2. **Python C-Extension (`symnmfmodule.c`)**  
   Exposes the C functions (`sym`, `ddg`, `norm`, `symnmf`) as Python-callable methods.  
   Handles safe conversion between Python lists and C data structures.

3. **Python Logic Layer**  
   - `symnmf.py`: Command-line runner for the algorithms.  
   - `analysis.py`: Implements **K-Means** and compares **silhouette scores**.  
   - `shared.py`: Handles file and parameter validation and initializes random **H** matrices.

---

## 🧠 Algorithms Implemented

### 1. Symmetric Non-Negative Matrix Factorization (SymNMF)
Approximates a normalized similarity matrix **W** as:
W ≈ H × H^T,  H ≥ 0

where **H** represents the soft cluster assignments.

**Key steps:**
- Initialize H with random positive values.
- Update iteratively:
  H_ij ← H_ij * sqrt( (W H)_ij / (H H^T H)_ij )
- Stop when change < ε or after 300 iterations.

---

### 2. K-Means Clustering
Implemented **from scratch** in Python:
- Initialize centroids from the first *k* points.  
- Assign each data point to its nearest centroid.  
- Update centroids as the mean of assigned points.  
- Repeat until convergence (centroid movement < ε).

---

## 📈 Evaluation
To measure clustering quality, both algorithms are evaluated using the **Silhouette Score** (`sklearn.metrics.silhouette_score`).  
Higher scores indicate better-defined clusters.

Example output:
```
nmf: 0.6251
kmeans: 0.6024
```

---

## 🧰 File Structure
```
├── analysis.py         # K-Means + silhouette comparison
├── shared.py           # Input validation and H initialization
├── symnmf.c            # Core matrix and SymNMF implementation in C
├── symnmf.h            # Header definitions for C
├── symnmfmodule.c      # C-Python binding layer
├── symnmf.py           # CLI runner for sym/ddg/norm/symnmf
├── setup.py            # Python extension build script
├── makefile            # Makefile to compile and clean
└── README.md           # Project documentation
```

---

## 🧪 How to Run

### 1. Build the C extension
```bash
make all
```

### 2. Run SymNMF from CLI
```bash
python3 symnmf.py 3 symnmf data/input.txt
```

### 3. Compare SymNMF vs K-Means
```bash
python3 analysis.py 3 data/input.txt
```

---

## 💡 Key Highlights

✅ Hybrid architecture combining **C efficiency** with **Python usability**  
✅ Full implementations of **SymNMF** and **K-Means** from scratch  
✅ Demonstrates numerical optimization, convergence logic, and matrix computation  
✅ Safe memory management and modular, documented code  
✅ Evaluation metrics integrated (Silhouette Score)  
✅ Automated build & execution pipeline via Makefile

---

## 👥 Authors

**Gad Rozen**  
**Hila Etzioni**

---

## 🏁 Summary

This project showcases algorithmic implementation, software design, and technical depth — bridging theoretical machine learning concepts with practical system development.  
It highlights teamwork, performance optimization, and code integration between multiple programming languages.

