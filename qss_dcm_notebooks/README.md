# QSS delayed-circuit-measurement (DCM) notebooks

These six notebooks are the DCM counterparts of the MCM package.

## DCM convention used here

- No syndrome or feed-forward measurement occurs inside the QSS recovery.
- No erased share is SWAPped directly into a fresh replacement register.
- The chosen erased/discarded shares stay in the quantum circuit as environment wires and are marked for discard.
- For the 5-qubit and Steane codes, a coherent recovery unitary is derived from the encoded logical basis and acts only on the surviving authorized set.
- For the qutrit code, the paper's unitary R12/R23/R31 recovery acts directly on the two surviving qutrits; the third share is left untouched.
- Optional discard diagnostics and all metric/tomography measurements are performed only after the unitary part of the circuit has finished.

The Steane notebooks retain selectable `ERASED_SHARES` and validate the full non-MDS access structure.  `RECOVERY_OUTPUT_SHARE=None` uses the paper's given output share when it survives and otherwise chooses the first surviving share.

All notebooks retain:
- ideal Aer simulation,
- noisy FakeBrisbane and FakeTorino options,
- optional M3 (`mthree`) final-readout mitigation,
- optimization-level selection,
- the SWAP-test overlap metric or entanglement-fidelity projector + tomography, as applicable.
