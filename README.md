# KZG Commitment Scheme Implementation

This code implements both the **Prover** and **Verifier** sides of the KZG polynomial commitment scheme using elliptic curve pairings over the BLS12-381 curve. The protocol allows for efficient polynomial commitment, evaluation proofs, and verification leveraging elliptic curve cryptography and bilinear pairings.

## Key Features:
- **Trusted Setup**: Generates public parameters, including the **commitment key (ck)** and **verification key (vk)**, to enable polynomial commitments up to a maximum degree `d`.
- **Polynomial Commitment**: The **Prover** commits to a polynomial `f(x)` without revealing its coefficients.
- **Evaluation Proof**: The Prover can compute and send a proof that `f(s) = z` for some value `s`. The proof involves creating a quotient polynomial.
- **Verification**: The **Verifier** checks the validity of the evaluation proof using the bilinear pairing properties of the elliptic curve.
- **Elliptic Curve Pairings**: Implements the BLS12-381 curve to compute pairings for secure verification.

## Classes and Functions:

### **Trusted_Setup**
Generates public parameters for the protocol.
- **`generate_public_parameters()`**: Generates the **commitment key (ck)** and **verification key (vk)**.

### **Prover**
Handles polynomial commitment and proof generation.
- **`commit(f)`**: Computes a commitment to a polynomial `f(x)` using the public parameters.
- **`eval(f, s)`**: Creates a quotient polynomial proof for the evaluation of `f(s)`.

### **Verifier**
Verifies the Prover’s commitment and evaluation proof.
- **`verify(c_f, s, z, c_q)`**: Verifies that the commitment `c_f` satisfies `f(s) = z` using the quotient commitment `c_q`.

## Tests:
The implementation includes three tests to validate the correctness of the KZG scheme:

1. **Manual Polynomial (Correct Commitment)**: Commits to a polynomial `f(x) = 1 + 2x + 3x^2` and verifies the evaluation at `s = 2`.
2. **Small Random Polynomial (Correct Commitment)**: Commits to a randomly generated polynomial and verifies the evaluation.
3. **Small Random Polynomial (Wrong Evaluation)**: Tests failure when the Prover provides an incorrect evaluation value.

### Test Scenarios:
```python
assert(manual_polynomial_correct_commitment() == True)
assert(small_random_polynomial_wrong_evaluation_commitment() == False)
assert(small_random_polynomial_correct_commitment() == True)
```

## Usage:
1. Install dependencies:
   ```
   pip install py_arkworks_bls12381 numpy
   ```
2. Set the maximum degree for polynomial commitments:
   ```python
   max_d = 100
   trusted_setup = Trusted_Setup(max_d)
   pp = trusted_setup.generate_public_parameters()
   ```
3. Commit and verify a polynomial:
   ```python
   f = P.Polynomial([1, 2, 3])  # Define polynomial
   prover = Prover(ck, f)
   verifier = Verifier(vk)

   c_f = prover.commit()
   s = 2 # The point that the prover is committing to.
   z = int(f(s))
   c_q = prover.eval(s)

   assert verifier.verify(c_f, s, z, c_q) == True
   ```

## References:
[KZG Polynomial Commitment tutorial](https://hackmd.io/769wh787T8SNaFwmNX74fA) by Dr. Anca Nitulescu.
