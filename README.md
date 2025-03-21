# KZG Commitment on Multiple Points

This implementation extends the KZG commitment scheme to support **batch evaluation proofs** for multiple points simultaneously, using techniques from [Aggregatable Subvector Commitments](https://alinush.github.io/2020/05/06/aggregatable-subvector-commitments-for-stateless-cryptocurrencies.html). The scheme leverages elliptic curve pairings over BLS12-381 for efficient cryptographic operations.

## Key Enhancements:
- **Batch Evaluation Proofs**: Prove polynomial evaluations at multiple points with a single proof (constant-sized proof).
- **Constant-Sized Verification**: Verification complexity remains O(1) regardless of points count.
- **Aggregation-Friendly**: Designed for efficient proof aggregation scenarios.
- **Subvector Commitments**: Support for proving polynomial evaluations on arbitrary subsets of points.

## Classes and Functions:
### **Trusted_Setup**
Generates public parameters for the protocol.
- **`generate_public_parameters()`**: Generates the **commitment key (ck)** and **verification key (vk)**.

### **Prover**
- **`commit(f)`**: Commits to polynomial `f(x)` using G1 group elements.
- **`eval(points)`**: Generates quotient proof for multiple evaluation points.
- **`quotient_poly(points)`**: Computes (f(x) - R_I(x)) / A_I(x) for vanishing poly A_I, and R_I is a polynomial s.t. R_I(s) = f(s) for all s in I.

### **Verifier**
- **`verify(commitment, points, values, proof)`**: 
  - Checks pairing equation: e(C - [R_I(τ)]_1, h) = e(π, [A_I(τ)]_2)

## Tests:
The code contains both unit tests and end-to-end tests.

### Unit Tests
Unit tests are used to test vanishing polynomial construction and quotient polynomial construction for small polynomials.

#### Test Scenarios:
```python
single_point_vanishing_polynomial_check()
two_points_vanishing_polynomial_check()
single_point_small_polynomial_quotient_check()
two_points_small_polynomial_quotient_check_1()
two_points_small_polynomial_quotient_check_2()
```

### End-to-End Tests
End-to-end tests are used to test the whole process (proving and verification).

#### Test Scenarios:
```python
multiple_point_manual_polynomial_correct_commitment_1()
multiple_point_manual_polynomial_correct_commitment_2()
multiple_point_small_random_polynomial_correct_commitment()
multiple_point_small_random_polynomial_wrong_evaluation_commitment()
```

## Usage:
1. Install dependencies:
   ```
   pip install py_arkworks_bls12381 numpy sympy
   ```
2. Set the maximum degree for polynomial commitments:
   ```python
   max_d = 100
   trusted_setup = Trusted_Setup(max_d)
   pp = trusted_setup.generate_public_parameters()
   ```
3. Commit and verify a polynomial:
   ```python
   f = P.Polynomial([3, 2, 1])  # Define polynomial (3x^2 + 2x + 1)
   prover = Prover(ck, f)
   verifier = Verifier(vk)

   polynomial_commitment = prover.commit()

   points = [1, 2] # The set of points the prover is committing to
   values = [int(polynomial_eval(f, point)) for point in points]
   
   quotient_polynomial_commitment = prover.eval(points)

   assert verifier.verify(polynomial_commitment, points, values, quotient_polynomial_commitment) == True
   ```
   
## References:
[Aggregatable Subvector Commitments for Stateless Cryptocurrencies from Lagrange Polynomials](https://alinush.github.io/2020/05/06/aggregatable-subvector-commitments-for-stateless-cryptocurrencies.html) by Alin Tomescu, Ittai Abraham, Vitalik Buterin, Justin Drake, Dankrad Feist, and Dmitry Khovratovich.

[KZG Polynomial Commitment tutorial (Exercise 1)](https://hackmd.io/769wh787T8SNaFwmNX74fA) by Dr. Anca Nitulescu.
