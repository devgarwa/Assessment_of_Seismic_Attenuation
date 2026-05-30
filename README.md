#  Assessment of Seismic Attenuation Parameters — Taranaki Basin, New Zealand

> **End-to-end geophysical data science pipeline:** signal processing, spectral analysis,
> statistical regression, non-linear optimisation, and interactive 3D visualisation
> applied to 170 km of real-world seismic survey data.

---

##  Project Overview

This project implements a complete data analytics and scientific computing pipeline to
extract seismic attenuation parameters from three large 2D seismic datasets acquired
over the offshore Taranaki Basin, New Zealand — the country's principal oil and gas
province.

Two independent spectral analysis methods are implemented in parallel, cross-validated
at survey intersection points, and supplemented by a non-linear dispersion inversion.
The project demonstrates the full data science lifecycle: raw data ingestion →
preprocessing → feature extraction → model fitting → validation → interactive
visualisation.

---

##  Technical Skills Demonstrated

| Area | Details |
|---|---|
| **Language** | Python 3 (Jupyter Notebooks) |
| **Signal Processing** | FFT, Hanning windowing, Hilbert transform envelope detection, spectral centroid computation |
| **Statistical Analysis** | Linear regression (OLS), R² goodness-of-fit, cross-validation at intersection tie points |
| **Optimisation / Curve Fitting** | Non-linear least-squares fitting (`scipy.optimize.curve_fit`) for Cole–Cole dispersion model |
| **Data Processing** | SEG-Y binary format parsing, large array manipulation, null-value interpolation, 2D Gaussian smoothing |
| **Pipeline Design** | Modular, multi-stage QC pipeline with 6 sequential filter stages |
| **Visualisation** | 2D heatmaps (Matplotlib), interactive 3D fence diagrams (Plotly, exported as HTML) |
| **Libraries** | NumPy, SciPy, ObsPy, Matplotlib, Plotly, Pandas |

##  Repository Structure
├── OR1P_Final.ipynb            # Full pipeline for Line OR1P (4,642 traces, 58 km)
├── OR2P_Final.ipynb            # Full pipeline for Line OR2P (4,480 traces, 56 km)
├── OR3P_Final.ipynb            # Full pipeline for Line OR3P (4,598 traces, 57.4 km)
├── Taranaki_3D_Interpretation.html   # Interactive 3D attenuation fence diagram
├── Taranaki_3D_Seismic_Model.html    # Interactive 3D seismic section model
└── Taranaki_3D_Solid.html            # Interactive 3D solid geological model


---

##  Methodology

### 1. Data Ingestion & Preprocessing
- Parsed SEG-Y binary seismic files using **ObsPy**
- Applied **Hilbert transform envelope detection** to automatically identify the
  seabed reflection on every trace (no manual picking)
- Applied **Hanning windowing** to suppress spectral leakage before FFT computation
- Selected 10–45 Hz analysis band based on global amplitude spectrum analysis

### 2. Ensemble Reference Spectrum
- Averaged amplitude spectra across all traces (seabed-centred 100 ms windows)
  to build a stable reference spectrum, eliminating trace-to-trace noise artefacts

### 3. Method 1 — Spectral Ratio (SR)
- Computed log-spectral ratio of target vs reference window for each (trace, depth)
  cell
- Applied **linear regression** (OLS) within the 10–45 Hz band; slope → Q estimate
- QC filter: accepted only cells with **R² ≥ 0.4** and physically valid Q range

### 4. Method 2 — Centroid Frequency Shift (CFS)
- Computed power-spectrum-weighted mean frequency (spectral centroid) for each window
- Estimated Q from centroid downshift using analytical formula:
  `Q = π × σ² × Δt / |Δfc|`
- More robust to noise than SR; validated by comparison at survey crossings

### 5. Cole–Cole Dispersion Inversion
- Fitted a **3-parameter Cole–Cole model** to the empirical Q⁻¹(f) curve at every
  accepted cell using `scipy.optimize.curve_fit`
- Applied 6-stage sequential QC pipeline:
  R² filter → frequency-in-band filter → broadness-exponent range →
  peak amplitude → spatial coherence → dual-method co-location

### 6. Cross-Validation
- At two survey intersection points, compared Q estimates from two independently
  processed lines — a rigorous data-driven validation not commonly done in practice
- CFS mis-ties: **< 2%** | SR mis-ties: **10–25%** → CFS confirmed as more reliable

### 7. 3D Visualisation
- Co-registered all three Q-map panels in geographic (NZTM) coordinate space
- Produced interactive HTML fence diagrams using **Plotly** for exploration

---

##  Key Results

| Metric | Value |
|---|---|
| Total seismic coverage processed | ~170 km across 3 lines |
| Total trace-time cells analysed | ~13,720 CDPs × depth windows |
| Identified attenuation anomaly depth | 3.36 s TWT (consistent on all 3 lines, ±40 ms) |
| Peak attenuation | 1/Q = 0.018–0.020 (Q ≈ 50–56) |
| CFS cross-validation mis-tie | < 2% |
| SR cross-validation mis-tie | 10–25% |
| Cole–Cole validated anomaly pixels | 784 (OR1P) / 1,503 (OR2P) / 771 (OR3P) |

---

## 🗺️ Interactive Visualisations

The three HTML files can be opened directly in any browser — no installation needed:

- **`Taranaki_3D_Interpretation.html`** — Annotated 3D attenuation fence diagram
  with all three seismic lines co-registered in geographic space
- **`Taranaki_3D_Seismic_Model.html`** — 3D seismic amplitude model
- **`Taranaki_3D_Solid.html`** — 3D solid geological model

---



> **Note:** The SEG-Y seismic data files are not included in this repository
> (commercial dataset). The notebooks are fully documented and all processing
> logic is self-contained and reproducible on any equivalent seismic dataset.

---


##  Background

Seismic attenuation (how quickly a seismic wave loses energy as it travels through
rock) is a measurable physical quantity sensitive to subsurface fluid content, porosity,
and rock type — information not available from conventional seismic amplitude analysis.
This project develops and validates a Python pipeline to extract this information from
standard 2D seismic survey data.

---

##  Author

**Devendra Garwa**
Integrated M.Tech., Geological Technology
Indian Institute of Technology Roorkee | 2026



---

*This project was completed as an M.Tech dissertation under the supervision of
Prof. Ravi Sharma, Department of Earth Sciences, IIT Roorkee.*

---

## 📂 Repository Structure
