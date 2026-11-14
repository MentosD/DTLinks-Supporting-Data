# DTLinks Supporting Data: Ground Motions & Soil-Structure Interaction (SSI) Model

This repository provides the supporting data for the paper, including:
1. Standardized 10-ms earthquake ground motion records (acceleration and displacement time histories).
2. The complete OpenSees numerical model file (`ssimodel.tcl`) representing the nonlinear soil-structure interaction system used in the real-time hybrid simulation (RTHS) and numerical validation studies.

---

## 1. Repository File Inventory

| File Name | Category | Description | Specifications |
| :--- | :--- | :--- | :--- |
| **`ssimodel.tcl`** | OpenSees Model | Full 2D Soil-Structure Interaction (SSI) model definition | Nonlinear fiber sections & multi-yield surface clay |
| **`Case_I_ElCentro_Accel_10ms.txt`** | Ground Motion | Ground Acceleration (1940 El Centro NS) | $\Delta t = 0.01\text{ s}$, 4418 pts, normalized to $1.000\text{ m/s}^2$ |
| **`Case_I_ElCentro_Disp_10ms.txt`** | Ground Motion | Ground Displacement (1940 El Centro NS) | $\Delta t = 0.01\text{ s}$, 4418 pts, Peak = $0.0678\text{ m}$ |
| **`Case_II_Datui_Accel_10ms.txt`** | Ground Motion | Ground Acceleration (Datui Earthquake) | $\Delta t = 0.01\text{ s}$, 7501 pts, normalized to $1.000\text{ m/s}^2$ |
| **`Case_II_Datui_Disp_10ms.txt`** | Ground Motion | Ground Displacement (Datui Earthquake) | $\Delta t = 0.01\text{ s}$, 7501 pts, Peak = $0.0779\text{ m}$ |

---

## 2. Soil-Structure Interaction (SSI) Model Details (`ssimodel.tcl`)

The `ssimodel.tcl` file provides the full finite element definition implemented in OpenSees:

### 2.1 Superstructure and Foundations (Material Nonlinearity)
- **Structural Configuration**: A 2-story, 2-bay reinforced concrete (RC) frame with underground single-column footings.
- **Element Formulation**: Displacement-based beam-column elements (`dispBeamColumn`) with 4 Gauss-Legendre integration points per element.
- **Section Discretization**: Fiber sections (`section Fiber`) capturing axial-flexural coupling and cross-sectional plastification.
- **Concrete Constitutive Model**: `uniaxialMaterial Concrete01` (Kent-Scott-Park model), explicitly accounting for confined core concrete ($f'_c = 34.5\text{ MPa}$) and unconfined cover concrete ($f'_c = 27.6\text{ MPa}$), with degraded linear softening and zero tensile strength.
- **Rebar Constitutive Model**: `uniaxialMaterial Hardening` with yield plateau and kinematic hardening ($f_y = 248.2\text{ MPa}$).
- **Rigid Diaphragms**: Floor slabs are constrained as rigid diaphragms (`rigidDiaphragm`) along the horizontal direction.

### 2.2 Foundation Soil (Multi-Yield Surface Plasticity)
- **Soil Domain Dimensions**: Horizontal width $32.4\text{ m}$ ($-9.2\text{ m}$ to $23.2\text{ m}$), depth $7.2\text{ m}$ ($-7.2\text{ m}$ to $0\text{ m}$), out-of-plane thickness $0.60\text{ m}$.
- **Element Formulation**: 2D 4-node plane-strain continuum elements (`element quadWithSensitivity`).
- **Soil Constitutive Model**: `nDMaterial MultiYieldSurfaceClay` (Prevost multi-yield-surface plasticity formulation), capturing nonlinear shear modulus reduction, hysteretic damping, and irreversible plastic strains under cyclic earthquake loading across four distinct soil layers.
- **Soil-Structure Interface**: Soil-foundation nodal degrees of freedom are coupled via kinematic constraints (`equalDOF`).

---

## 3. Earthquake Ground Motion Records

- **Sampling Rate**: $100\text{ Hz}$ ($\Delta t = 0.010\text{ s} = 10\text{ ms}$).
- **File Format**: ASCII plain text, single column, headerless.
- **Units**:
  - Acceleration (`*_Accel_10ms.txt`): $\text{m/s}^2$
  - Displacement (`*_Disp_10ms.txt`): $\text{m}$
- **Scaling in Studies**:
  - The baseline acceleration records are normalized to a peak of $1.0\text{ m/s}^2$.
  - In the paper's OpenSees dynamic analyses and RTHS tests, an amplitude factor of **$1.47$** was applied ($\text{PGA} = 1.47\text{ m/s}^2 \approx 0.15\text{ g}$).
  - In ground motion visualization plots, records were windowed/padded (5 s leading zeros) and scaled to $0.10\text{ g}$.
