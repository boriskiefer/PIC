# Electro-Optical PIC Mach–Zehnder Interferometer

**From waveguide supermodes to a quantum optical unitary.** This executable notebook and technical brief demonstrate how physical parameters of a photonic integrated circuit (PIC) determine directional-coupler transformations, which combine with electro-optic phase shifts to reproduce the corresponding free-space Mach–Zehnder interferometer (MZI).

## Files

- [**sim_PIC_MZI_v2.ipynb**](sim_PIC_MZI_v2.ipynb) — interactive PIC simulator and numerical validation benchmarks.
- [**MZI_Technical_Brief.pdf**](MZI_Technical_Brief.pdf) — step-by-step theory, worked example, numerical verification, and reading resources.
- [**requirements.txt**](requirements.txt) — Python dependencies for this simulator.

[← PIC repository overview](../README.md)

## Optical operation order

The **free-space** convention is exactly

**PRE → BS1 → INTERNAL → BS2**

and the **PIC** realization is

**PRE → DC1 → INTERNAL EO → DC2**.

Both PRE and INTERNAL are phase operations on the **upper** rail. With column-vector amplitudes, the rightmost matrix acts first:

\[
U_{\mathrm{PIC}} = C_2\,P_{\mathrm{internal}}(\phi)\,C_1\,P_{\mathrm{pre}}(\chi).
\]

The physical directional-coupler matrices are derived from even/odd supermode propagation, not assigned independently as abstract quantum gates. With matched beamsplitter port-phase conventions, the free-space and PIC matrices agree (up to a physically irrelevant global phase where appropriate).

## Physical model

The notebook accepts waveguide width, core and cladding indices, wavelength, separation gap and coupling length for each coupler, input pre-phase, and internal modulator voltage, half-wave voltage, and bias. A **scalar, symmetric, one-dimensional slab-waveguide eigenproblem** provides the even and odd propagation constants. The model uses

\[
\kappa = \frac{\beta_{\mathrm{even}}-\beta_{\mathrm{odd}}}{2},\qquad
C(L)=\begin{pmatrix}\cos(\kappa L)&-i\sin(\kappa L)\\-i\sin(\kappa L)&\cos(\kappa L)\end{pmatrix}.
\]

The phase shifters use diagonal upper-rail phase matrices, with the internal phase parameterized by \(\phi=\phi_{\mathrm{bias}}+\pi V/V_\pi\). The interface displays **each component matrix, its cumulative product, single-photon amplitudes, and output detection probabilities**, as well as the final composite unitary and a global-phase-corrected Hadamard comparison.

## Install and run

From this `MZI/` directory:

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
python -m pip install -r requirements.txt
jupyter notebook sim_PIC_MZI_v2.ipynb
```

Execute the notebook from top to bottom. Use **Set exact Hadamard reference** to compute the balanced coupling lengths from the current geometry. Run the notebook's **Numerical validation benchmarks** cell; assertions should pass.

## Reproducible reference settings

| Parameter | Value |
|---|---:|
| Waveguide width | 0.75 µm |
| Core / cladding indices | 1.50 / 1.45 |
| Wavelength | 1.55 µm |
| Coupler gaps (DC1 and DC2) | 0.35 µm |
| Coupler lengths (DC1 and DC2) | 15.260033195 µm (calculated 50:50 value) |
| Internal phase bias | 0 rad |
| Modulator half-wave voltage | 5.0 V |

For this idealized model, \(\kappa\approx0.051467658\ \mu\mathrm m^{-1}\) and \(\kappa L=\pi/4\).

| Benchmark | Input pre-phase \(\chi\) | Internal voltage | Internal phase \(\phi\) | Expected composite |
|---|---:|---:|---:|---|
| Hadamard | 0 | −2.5 V | −π/2 | \(e^{-3\pi i/4}H\) |
| Identity | π | +5.0 V | π | \(I\) |

For the Hadamard case, \(H=\frac1{\sqrt2}\begin{pmatrix}1&1\\1&-1\end{pmatrix}\). The simulator's composite matrix differs from \(H\) by **global phase**, not by a failure of unitarity. A rounded slider length of `15.2600` µm produces a small nonzero Hadamard residual (approximately `3.42e-6` in the previously reported run), while the composite remains unitary to numerical precision. Use the exact-reference preset for the tightest matrix comparison.

## Validation and limits

The notebook checks unitarity, normalization, component ordering, phase behavior, analytical MZI interference, and target-unitary agreement. The companion [technical brief](MZI_Technical_Brief.pdf) derives the electromagnetic supermode basis, the basis transformation \(S\), propagation \(D(L)\), and the two-mode coupler \(C(L)=S D(L) S^\dagger\).

**Limits:** ideal monochromatic, lossless, single-photon two-mode propagation; scalar 1D slab geometry rather than full-vector 3D cross sections; no fabrication tolerance, loss, multimode effects, photon distinguishability, or material-specific EO calibration. The demonstrated equivalence concerns the ideal quantum optical transformation, not a unique physical layout for every free-space circuit.

## Contact

Boris Kiefer · New Mexico State University  
[GitHub](https://github.com/boriskiefer) · [LinkedIn](https://www.linkedin.com/in/boris-kiefer-85089831/)

Confirm license and attribution before public release.

