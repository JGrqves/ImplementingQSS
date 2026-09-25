# Implementing Quantum Secret Sharing (with Qiskit)

This repository contains Qiskit implementations and analysis code for the quantum secret-sharing (QSS) schemes studied in **“Implementing Quantum Secret Sharing on Current Hardware”** (`arXiv:2410.11640v3`).

The implementations cover the `[[5,1,3]]` five-qubit QSS scheme, the `[[7,1,3]]` Steane-code scheme including its non-MDS access structure, and the `((2,3))` three-qutrit scheme embedded into qubits. Both mid-circuit-measurement (MCM) and delayed-circuit-measurement (DCM) variants are included, together with SWAP-test state-overlap and entanglement-fidelity measurements, ideal/noisy simulation, and optional M3 readout mitigation.

## Repository layout

```text
.
├── qss_mcm_notebooks/             # MCM QSS notebooks
├── qss_dcm_notebooks/             # DCM / fully coherent QSS notebooks
├── QSS_Plots.ipynb                # analysis and paper-style plots
├── QSS_Data/                      # source Excel workbooks
│   ├── MCM_SIM.xlsx
│   ├── MCM_IBMQ.xlsx
│   ├── DCM_SIM.xlsx
│   ├── DCM_IBMQ.xlsx
│   ├── ENT_FID_MCM_SIM.xlsx
│   ├── ENT_FID_MCM_IMBQ.xlsx
│   ├── ENT_FID_DCM_SIM.xlsx
│   ├── ENT_FID_DCM_IBMQ.xlsx
│   ├── Steane3Erasures_SIM_IBMQ_MCM.xlsx
│   └── Steane3Erasures_ENT_FID_SIM_MCM.xlsx
├── README.md
└── requirements.txt
```

`QSS_Plots.ipynb` uses exactly:

```python
QSS_DATA_DIR = Path.cwd() / "QSS_Data"
```

## Installation

Python 3.10+ is recommended.

```bash
python -m venv .venv
source .venv/bin/activate        # macOS/Linux
# .venv\Scripts\activate         # Windows

python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter lab
```

### LaTeX note

The plotting notebook enables Matplotlib's `usetex=True`, and some Qiskit circuit drawers also use LaTeX. A working TeX installation may therefore be required for identical rendering. If TeX is unavailable, the plotting notebook can be changed to `usetex=False`, but text rendering will differ slightly.

## MCM and DCM notebooks

The MCM qubit-code notebooks implement syndrome measurement and classical feed-forward recovery.

The DCM qubit-code notebooks keep the recovery coherent. Selected shares are marked for discard rather than being directly SWAPped into fresh erasure registers, and measurements are delayed until the end.

The qutrit implementation uses the two-qubit qutrit embedding and the corresponding `R12`, `R23`, or `R31` unitary recovery.

For the Steane code, erased/discarded shares are selectable before circuit construction and are validated against its general (non-MDS) access structure.

## Backends and readout mitigation

The QSS notebooks support:

- `ideal` — noiseless Aer simulation,
- `fake_brisbane` — noisy FakeBrisbane simulation,
- `fake_torino` — noisy FakeTorino simulation.
- *Must set up your own (IBM Quantum) authentication for access to actual quantum hardware*

When `USE_M3 = True`, `mthree` is applied to the final measured distributions and the mitigated quasi-distribution is mapped to a physical probability distribution before the reported metric is calculated.

## Data and plotting notebook

`QSS_Plots.ipynb` reads the experimental/simulation arrays directly from the `arrays` sheet of the Excel workbooks in `QSS_Data/`.

### Circuit depth / gate-count figures

QSS_data contains the SWAP-test and entanglement-fidelity datasets. They do not contain separate Excel datasets for the circuit-depth and gate-count constants used by the final three plotting cells.

## Reproducing the plots

Run Jupyter from the repository root so that `Path.cwd() / "QSS_Data"` resolves to the extracted data directory:

```bash
jupyter lab
```

Then open `QSS_Plots.ipynb` and run the cells from top to bottom.

## Citation

If you use this repository, please cite the associated paper:

```bibtex
@article{graves_qss_hardware,
        title={Implementing Quantum Secret Sharing on Current Hardware},
        author={Jay Graves and Mike Nelson and Eric Chitambar},
        year={2025},
        eprint={2410.11640},
        archivePrefix={arXiv},
        primaryClass={quant-ph},
        url={https://arxiv.org/abs/2410.11640},
}
