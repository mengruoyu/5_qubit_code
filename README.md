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
