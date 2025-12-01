
<h1 align="center">Reed Solomon Code <br> Implementation using Python</h1>

This project implements a Reed–Solomon encoder/decoder from scratch using **finite field GF(256)** arithmetic, including a Berlekamp–Massey + Chien Search + Forney implementation for error/erasure correction.

---

## 🔧 Features

### ✔ Reed–Solomon Encoder
- Polynomial-based encoding over GF(256)
- Generator polynomial constructed using primitive polynomial **0x11D**
- Highest-first polynomial representation (standard RS form)

### ✔ Reed–Solomon Decoder (Classical Algebraic Method)
Implements the full decode pipeline:

1. **Syndrome Computation**  
   Evaluates the received polynomial at successive powers of the primitive element α.

2. **Berlekamp–Massey Algorithm**  
   Produces the error locator polynomial by fitting the syndrome sequence.

3. **Chien Search**  
   Efficient root-finding over GF(256) to determine exact error positions.

4. **Forney Algorithm**  
   Computes error magnitudes using the evaluator polynomial and the derivative of the locator polynomial.

The decoder fully reconstructs the original codeword as long as errors ≤ correction capability.

---

## 📂 Project Structure
```
reed-solomon/
│
├── rs_codec/
│   │
│   ├── __init__.py               # Makes rs_codec a Python package
│   ├── encoder.py                # High-level encoder wrapper
│   ├── generator.py              # Generator polynomial construction
│   ├── gf.py                     # GF(256) finite field implementation
│   ├── poly.py                   # Polynomial helper utilities
│
├── rs_encoder.py                 # Full RS encoding logic (uses gf & poly) 
├── rs_bm_forney.py               # Decoding: BM, Chien, Forney algorithms
├── test_encode.py                # Tests the encoding process
├── test_full_cycle_bm.py         # Full encode → corrupt → decode test
│
├── README.md                     # Project documentation
├── .gitattributes                # Git attributes handling
└── .gitignore                    # Files/directories ignored by Git

```


## 📘 Overview
- A Reed–Solomon code is commonly described by parameters $(n,k)$ with $t = \frac{n-k}{2}$ the error-correcting capability.
- Over GF(2<sup>m</sup>), codeword length satisfies: 
<div align="center">
<b>n ≤ 2<sup>m</sup> − 1</b>
</div>
- Generator polynomial (conceptual):
<div align="center">
g(x) = &prod;<sub>i=0</sub><sup>2t−1</sup> (x − &alpha;<sup>i</sup>)
</div>

## 🧮 Finite Field (GF256) Implementation

- Field polynomial: **0x11D**
- Primitive element: α = 2  
- exp/log tables for O(1) GF multiplication  
- Addition/subtraction implemented as XOR  

This matches standard Reed–Solomon implementations used in:
- CDs/DVDs  
- QR codes  
- Digital TV (DVB)  
- Satellite communication  

---

## 🚀 Example: Full Encode → Corrupt → Decode Cycle

Run:

```bash
python3 test_full_cycle_bm.py
```

## 🤝 Contributing

Contributions are welcome! Fork this repo and submit a pull request.

---

## 🛠 Requirements

- Python 3.8+
- No external libraries required — pure Python implementation.

---




