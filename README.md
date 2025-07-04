# Qiskit Realization of 5-qubit error correcting code
We realize the 5-qubit error correcting code in this project, which we refer to https://en.wikipedia.org/wiki/Five-qubit_error_correcting_code

## Usage

```python 
import numpy as np, random
from qiskit import AerSimulator

p_error = 0.1
x_encoded = 1
qc        = five_qubit_ECC(p_error, x_encoded)
backend = AerSimulator()              # supports mid-circuit measurement
job     = backend.run(qc, shots=1, memory=True)
```
# Quantum Computing

## What is an error and why error correction

In a quantum circuit, an error is anything that happens to a qubit other than the circuit's instruction.

In reality, a physical qubit is very noisy so that, without error correction, an algorithm cannot be performed.

Tranditional fault-tolorant strategy does not work. For example, adding a check-point is no-go in quantum computing because of the no-cloning theorem. Therefore, we need to use error-correcting code.

## Quantum error correcting code

The quantum error correcting code (ECC) encodes a logical qubit $| \psi \rangle $ into  a pure state represented by a superposition of $n$ physical qubits such that if $<d/2$ physical qubits encounter error, one still can recover the original logical qubit $| \psi \rangle $ (error correction).

This project considers the 5-qubit ECC. It is the smallest possible quantum error-correcting code that can detect and correct an arbitrary single-qubit error.

| **Ingredient** | **What it is / does** | **Comment** |
|---------------|-----------------------|-------------|
| **Logical codewords** | Two highly entangled 5-qubit states, usually written \|0⟩<sub>L</sub>, \|1⟩<sub>L</sub> | — |
| **Stabilizers** | 4 independent Pauli operators  <br> g₁ = **X Z Z X I**  <br> g₂ = **I X Z Z X**  <br> g₃ = **X I X Z Z**  <br> g₄ = **Z X I X Z** | Measuring each *gᵢ* gives a ±1 **syndrome** bit. Together the 4-bit syndrome pinpoints which Pauli error (if any) hit which qubit. |
| **Logical operators** | **X<sub>L</sub> = X X X X X**  <br> **Z<sub>L</sub> = Z Z Z Z Z** | They commute with the stabilizers but flip the logical state. Acting on ≥ 3 qubits is why the code distance is 3. |



### Error Detection & Correction Cycle (Conceptual)

1. **Encode** – Map one bare qubit to five physical qubits using a fixed circuit (lots of CNOTs & Hadamards).  
2. **Let the computation run** – The environment may apply an **X**, **Y**, or **Z** error on any one of the five physical qubits.  
3. **Syndrome measurement** – Measure the four stabilizers (requires ancilla qubits and CNOTs, but *does not* collapse the logical state).  
4. **Syndrome lookup** – Each single-qubit Pauli error yields a unique ± ± ± ± pattern.  
5. **Correct** – Apply the appropriate Pauli operator to undo the error (or update the Pauli frame).  
6. **Resume** – The logical qubit is restored; keep computing or decode when finished.


