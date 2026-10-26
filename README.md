# PIC — Quantum Photonic Integrated Circuits

**From quantum-information operations to physically parameterized photonic integrated circuits.**

This repository collects executable simulators and companion technical briefs that connect free-space quantum optical circuits with their integrated photonic realizations. Each implementation documents the physical model, the component-level transformations, the composite operation, and reproducible validation benchmarks.

## Simulators

| Implementation | Scope | Simulator and documentation |
|---|---|---|
| **Electro-optical Mach–Zehnder interferometer (MZI)** | Scalar coupled-waveguide supermodes; two directional couplers; upper-rail input and internal phase shifts; free-space/PIC matrix comparison; Hadamard and identity benchmarks | [MZI/](MZI/) |

## Getting started

The simulators are self-contained in their own subdirectories. For the MZI:

```bash
cd MZI
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
python -m pip install -r requirements.txt
jupyter notebook sim_PIC_MZI_v2.ipynb
```

See the [MZI README](MZI/README.md) for model assumptions, exact optical operation order, benchmark settings, and validation instructions.

## Scope and interpretation

The emphasis is on **quantitative correspondence between target quantum operations and physical photonic components**, not fabrication-ready engineering specifications. Each simulator's domain of validity and approximations are stated in its own documentation.

## Author

Boris Kiefer · New Mexico State University  
[GitHub](https://github.com/boriskiefer) · [LinkedIn](https://www.linkedin.com/in/boris-kiefer-85089831/)

## License

A software license has not yet been selected. Confirm attribution and release permissions before publication.
