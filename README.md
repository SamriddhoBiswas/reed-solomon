
# Reed–Solomon Code (Python)

This project implements a Reed–Solomon encoder/decoder and supporting finite-field utilities in pure Python, including a Berlekamp–Massey + Forney implementation for error/erasure correction.

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
reed-solomon/
│
├── rs_codec/
│   │
│   ├── __pycache__/              # Python bytecode cache
│   │
│   ├── __init__.py               # Makes rs_codec a Python package
│   ├── encoder.py                # High-level encoder wrapper
│   ├── generator.py              # Generator polynomial construction
│   ├── gf.py                     # GF(256) finite field implementation
│   ├── poly.py                   # Polynomial helper utilities
│   │
│   ├── rs_encoder.py             # Full RS encoding logic (uses gf & poly)
│   ├── rs_bm_forney.py           # Decoding: BM, Chien, Forney algorithms
│   │
│   ├── test_encode.py            # Tests the encoding process
│   └── test_full_cycle_bm.py     # Full encode → corrupt → decode test
│
├── README.md                     # Project documentation
├── .gitattributes                # Git attributes handling
└── .gitignore                    # Files/directories ignored by Git


Overview
- Implements Reed–Solomon encoding and decoding primitives over GF(2^m).
- Implements polynomial utilities and generator polynomial computation.
- Includes a Berlekamp–Massey solver and Forney algorithm for decoding (see [rs_bm_forney.py](rs_bm_forney.py)).
- Contains unit tests to validate encoding and full encode/decode cycles.

Reed–Solomon basics
- A Reed–Solomon code is commonly described by parameters $(n,k)$ with $t = \frac{n-k}{2}$ the error-correcting capability.
- Over GF$(2^m)$, codeword length satisfies:
$$
n \le 2^m - 1
$$
- Generator polynomial (conceptual):
$$
g(x)=\prod_{i=0}^{2t-1} (x - \alpha^{i})
$$

Usage
- Run the encoder directly or import the encoder module:

```python
# example usage (place in a script or REPL)
from rs_encoder import encode  # or use rs_codec.encoder

msg = [32, 45, 12, 255]  # example message symbols (0..255 for GF(2^8))
codeword = encode(msg)
print(codeword)
```

Testing
- Run tests with pytest:

```sh
pytest -q
# or run specific tests
pytest test_encode.py -q
pytest test_full_cycle_bm.py -q
```

Project structure
- [rs_codec/gf.py](rs_codec/gf.py) — finite field arithmetic (addition, multiplication, log/antilog tables).
- [rs_codec/poly.py](rs_codec/poly.py) — polynomial operations (add, mul, eval, scale).
- [rs_codec/generator.py](rs_codec/generator.py) — generator polynomial construction.
- [rs_codec/encoder.py](rs_codec/encoder.py) and [rs_encoder.py](rs_encoder.py) — public encoder interfaces.
- [rs_bm_forney.py](rs_bm_forney.py) — Berlekamp–Massey solver + Forney algorithm for decoding and error/erasure location/value computation.
- Tests: [test_encode.py](test_encode.py), [test_full_cycle_bm.py](test_full_cycle_bm.py)

Contributing
- Open an issue for bugs or feature requests.
- Preferred workflow: fork → branch → PR with tests.

License
- Add an appropriate license file (e.g., MIT) if you plan to release publicly.

Notes
- This README intentionally avoids committing to particular API symbol names — refer to the modules listed above for the exact function/class names.
- For algorithmic details, see standard references on Reed–Solomon, Berlekamp–Massey, and Forney algorithms.


// ...existing code...