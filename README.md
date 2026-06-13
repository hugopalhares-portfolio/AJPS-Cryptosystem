# The Mersenne Number Public-Key Cryptosystem (AJPS)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Topic](https://img.shields.io/badge/topic-post--quantum%20cryptography-7b3fe4)

A from-scratch implementation and analysis of the post-quantum public-key
cryptosystem proposed by **Aggarwal, Joux, Prakash and Santha (CRYPTO 2018)**,
whose security rests on low-Hamming-weight arithmetic modulo a **Mersenne prime**
$p = 2^n - 1$ rather than on polynomial rings.

This was my individual project for the *Cryptography and Security Protocols*
course (MSc Data Science and Engineering, Instituto Superior Técnico). The entire
work lives in a single, self-contained Jupyter notebook that builds the scheme up
from its number-theoretic foundations, demonstrates it end to end, and then
analyses its correctness, security and performance.

## What it implements

- **AJPS-1** — the single-bit scheme: key generation $H = F \cdot G^{-1} \bmod p$
  from low-weight secrets, encryption via $A\cdot H + B$, and Hamming-weight
  threshold decryption.
- **AJPS-2** — the blockwise scheme with **repetition-code error correction**,
  which the paper introduces to make the construction practical.
- **Byte-level messaging** on top of the bit-level primitive (encrypt / decrypt
  arbitrary `bytes`).
- **Correctness analysis** — empirical decryption error rate as a function of the
  parameters $(n, h)$, and the boundary condition $n > 4h^2$.
- **Security analysis** — the brute-force baseline and, crucially, the
  **Beunardeau et al. lattice attack** that breaks weak instances with LLL in
  dimension 2, reducing security to $\approx 2h$ bits and forcing the parameter
  choice $h \gtrsim \lambda/2$, $n > 4h^2$.
- **Performance benchmarks** — timing key generation, encryption and decryption
  at realistic Mersenne exponents (e.g. $n = 216091$ at the 128-bit level).

## Notebook structure

| Section | Contents |
|---|---|
| 1 | Motivation: why post-quantum, why Mersenne primes |
| 2–3 | Hamming weight and the rotation / addition / multiplication / negation bounds the scheme relies on |
| 4–6 | The AJPS-1 scheme, a worked small example, and decryption |
| 7 | Correctness boundary and empirical error-rate measurement |
| 8 | Ciphertext-size blow-up (the motivation for the blockwise scheme) |
| 9 | Security: brute-force baseline, the lattice attack, and parameter consequences |
| 10 | The blockwise AJPS-2 scheme with repetition coding |
| 11 | Performance benchmarks at cryptographic parameter sizes |

> **Note:** this is an educational implementation built to understand and analyse
> the scheme. It is **not** constant-time or otherwise hardened, and should not be
> used to protect real data.

## Running it

```bash
git clone https://github.com/hugopalhares-portfolio/AJPS-Cryptosystem.git
cd ajps-mersenne-cryptosystem
pip install -r requirements.txt
jupyter notebook ajps_cryptosystem.ipynb
```

The notebook ships with its outputs saved, so it can also be read straight through
on GitHub without running anything. Only `matplotlib` is required; everything else
(`secrets`, `random`, `math`, `time`) is in the Python standard library.

## References

1. D. Aggarwal, A. Joux, A. Prakash, M. Santha. *A New Public-Key Cryptosystem via
   Mersenne Numbers.* CRYPTO 2018, LNCS 10993, pp. 459–482.
2. M. Beunardeau, A. Connolly, R. Géraud, D. Naccache. *On the Hardness of the
   Mersenne Low Hamming Ratio Assumption.* IACR ePrint 2017/522.
3. J.-S. Coron, A. Gini. *Improved Cryptanalysis of the AJPS Mersenne Based
   Cryptosystem.* IACR ePrint 2019/610; J. Mathematical Cryptology, 2020.
