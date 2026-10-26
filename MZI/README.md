# Electro-Optical PIC Mach–Zehnder Interferometer

**From waveguide supermodes to a quantum optical unitary.**

This project demonstrates how the physical parameters of a photonic integrated circuit (PIC) determine the directional-coupler transformations and how those transformations combine with electro-optic phase shifts to implement a free-space Mach–Zehnder interferometer (MZI). The accompanying Jupyter notebook shows the matrix and single-photon output probabilities **after every component**.

[← Back to the PIC repository](../README.md)

## Files

| File | Contents |
|---|---|
| [`sim_PIC_MZI_v2.ipynb`](sim_PIC_MZI_v2.ipynb) | Interactive simulator and numerical benchmarks |
| [`MZI_Technical_Brief.pdf`](MZI_Technical_Brief.pdf) | Derivations, worked examples, validation, and references |
| [`requirements.txt`](requirements.txt) | Python dependencies |

## Physical circuit and operation order

The **free-space** operation order is:

**PRE → BS1 → INTERNAL → BS2**

The corresponding **PIC** operation order is:

**PRE → DC1 → INTERNAL EO → DC2**

PRE and INTERNAL are both phase shifters on the **upper rail**. With column-vector amplitudes, operations act from right to left:

$$
U_{\mathrm{PIC}}=C_2\,P_{\mathrm{internal}}(\phi)\,C_1\,P_{\mathrm{pre}}(\chi).
$$

Here $C_1$ and $C_2$ are the two directional-coupler matrices, $\chi$ is the input pre-phase, and $\phi$ is the internal electro-optic phase. Matching the free-space beamsplitter port-phase convention gives the same two-mode optical transformation (allowing an overall global phase).

## From waveguide geometry to a coupler matrix

The simulator models symmetric, coupled slab waveguides. A scalar one-dimensional electromagnetic eigenproblem determines the propagation constants of the **even** and **odd** supermodes. Their splitting sets the directional-coupler strength:

$$
\kappa=\frac{\beta_{\mathrm{even}}-\beta_{\mathrm{odd}}}{2}.
$$

After removing the common propagation phase, the coupler matrix is

$$
C(L)=
\begin{pmatrix}
\cos(\kappa L) & -i\sin(\kappa L) \\
-i\sin(\kappa L) & \cos(\kappa L)
\end{pmatrix}.
$$

The technical brief derives this matrix by diagonalizing the coupled-mode propagation operator and transforming between the **waveguide** and **supermode** bases. It is not an independently assigned quantum gate.

For the internal phase shifter,

$$
\phi=\phi_{\mathrm{bias}}+\pi\frac{V}{V_\pi},
\qquad
P_{\mathrm{internal}}(\phi)=
\begin{pmatrix}e^{i\phi}&0\\0&1\end{pmatrix}.
$$

The pre-phase shifter has the same diagonal form with $\chi$ in place of $\phi$.

## Install and run

Run the following commands **from the `MZI/` directory**:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter notebook sim_PIC_MZI_v2.ipynb
```

**Windows PowerShell:** replace the `source` command with `.venv\Scripts\Activate.ps1`.

Execute the notebook cells from top to bottom. The interface updates the **individual component matrices**, **cumulative matrix products**, **single-photon amplitudes**, **detection probabilities**, and **final composite unitary** when parameters change.

Use **Set exact Hadamard reference** to calculate balanced coupling lengths from the selected waveguide geometry. Run the **Numerical validation benchmarks** cell to check the implementation.

## Reproducible reference geometry

| Physical parameter | Value |
|---|---:|
| Waveguide width | 0.75 µm |
| Core refractive index | 1.50 |
| Cladding refractive index | 1.45 |
| Wavelength | 1.55 µm |
| Gap, DC1 and DC2 | 0.35 µm |
| Coupling length, DC1 and DC2 | 15.260033195 µm (50:50) |
| Internal phase bias | 0 rad |
| Half-wave voltage, $V_\pi$ | 5.0 V |

For this geometry, the simulator obtains approximately $\kappa=0.051467658\;\mu\mathrm{m}^{-1}$ and $\kappa L=\pi/4$.

### Target-unitary benchmarks

| Target | Input pre-phase $\chi$ | Internal voltage $V$ | Internal phase $\phi$ | Composite unitary |
|---|---:|---:|---:|---|
| Hadamard | 0 | −2.5 V | $-\pi/2$ | $U_{\mathrm{PIC}}=e^{-3\pi i/4}H$ |
| Identity | $\pi$ | +5.0 V | $\pi$ | $U_{\mathrm{PIC}}=I$ |

The standard Hadamard is

$$
H=\frac{1}{\sqrt{2}}
\begin{pmatrix}
1&1\\
1&-1
\end{pmatrix}.
$$

The Hadamard result matches the full complex matrix **up to global phase**, not merely its output probabilities. For the rounded length `15.2600` µm, a previously reported run produced a phase-corrected Frobenius residual of about `3.42e-6`; the **exact reference preset** gives a substantially tighter match. Matrix unitarity remains satisfied to numerical precision.

## Validation and model boundaries

The notebook checks component and composite unitarity, probability normalization, phase behavior, component ordering, analytical interference, and the Hadamard and identity targets.

**Scope:** lossless, monochromatic, two-mode single-photon propagation; symmetric scalar 1D slab-waveguide model. This is a physical-to-quantum proof of concept, **not** a fabrication-ready full-vector 3D PIC design. The model does not include propagation loss, fabrication tolerances, higher-order modes, photon distinguishability, or material-specific modulator calibration.

The equivalence concerns **quantum optical transformations**, not a unique one-to-one mapping of every free-space hardware geometry to a PIC layout.

## Contact

Boris Kiefer · New Mexico State University  
[GitHub](https://github.com/boriskiefer) · [LinkedIn](https://www.linkedin.com/in/boris-kiefer-85089831/)
