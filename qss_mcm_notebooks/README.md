# QSS Qiskit notebooks

Six notebooks for the paper's 5-qubit, 7-qubit Steane, and embedded 3-qutrit QSS circuits for the SWAP-test/state-overlap and entanglement-fidelity metrics.

## New execution controls

Each notebook has a top-level `BACKEND_MODE` selector:

- `"ideal"` — noiseless `AerSimulator`.
- `"fake_brisbane"` — noisy Aer simulator generated from `qiskit_ibm_runtime.fake_provider.FakeBrisbane`.
- `"fake_torino"` — noisy Aer simulator generated from `FakeTorino`.

The paper compared Brisbane hardware/noisy FakeBrisbane and also FakeTorino. The fake snapshots provide the device basis, connectivity and error parameters; `AerSimulator.from_backend(...)` performs the actual local noisy simulation.

## M3 / mthree

Set `USE_M3 = True` to perform M3 readout-error mitigation. The notebooks:

1. transpile first,
2. obtain the physical-qubit mapping of the final measurements,
3. calibrate M3 on those physical qubits,
4. apply M3 to the raw histogram, and
5. convert the quasi-distribution to its nearest physical probability distribution before computing the reported probability/fidelity.

M3 is applied to the final metric/tomography measurements. It cannot retroactively correct a mid-circuit syndrome bit that already controlled feed-forward during the same shot.

For entanglement fidelity, the notebooks now include an explicit X/Y/Z Pauli-tomography implementation (3^n settings) so M3 can be applied to every tomography histogram before linear-inversion reconstruction. The direct Bell/Φ projector check is retained as a useful sanity check.

## Access structure

- The 5-qubit code accepts any erased set of size at most 2.
- The qutrit threshold scheme lets you choose which one of the 3 qutrit shares is discarded.
- The Steane notebooks expose `ERASED_SHARES` and validate it against the non-MDS access structure, including the additional correctable 3- and 4-erasure patterns.

## Dependencies

```text
qiskit>=2.0
qiskit-aer>=0.17
qiskit[visualization]
qiskit-ibm-runtime
mthree>=3.0
matplotlib
```
