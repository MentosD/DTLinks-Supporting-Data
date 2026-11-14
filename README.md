# DTLinks Supporting Data: Ground Motion Records

This repository provides the earthquake ground motion input data (acceleration and displacement time histories) used in the real-time hybrid simulation (RTHS) experiments and OpenSees numerical simulations for the paper.

---

## 1. File Inventory and Description

| File Name | Record / Event | Data Type | Time Step ($\Delta t$) | Number of Points | Duration | Peak Value |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`Case_I_ElCentro_Accel_10ms.txt`** | El Centro (1940 Imperial Valley, NS) | Ground Acceleration | 0.010 s (10 ms) | 4418 | 44.17 s | Normalized to $1.000\text{ m/s}^2$ |
| **`Case_I_ElCentro_Disp_10ms.txt`** | El Centro (1940 Imperial Valley, NS) | Ground Displacement | 0.010 s (10 ms) | 4418 | 44.17 s | $0.0934\text{ m}$ |
| **`Case_II_Datui_Accel_10ms.txt`** | Datui Earthquake Record | Ground Acceleration | 0.010 s (10 ms) | 7501 | 75.00 s | Normalized to $1.000\text{ m/s}^2$ |
| **`Case_II_Datui_Disp_10ms.txt`** | Datui Earthquake Record | Ground Displacement | 0.010 s (10 ms) | 7501 | 75.00 s | $0.1554\text{ m}$ |

---

## 2. File Format

- **Format**: Plain ASCII text (`.txt`), single column, no header.
- **Sampling Interval**: $\Delta t = 0.010\text{ s}$ ($10\text{ ms}$, sampling rate = $100\text{ Hz}$).
- **Units**:
  - Acceleration files (`*_Accel_10ms.txt`): $\text{m/s}^2$
  - Displacement files (`*_Disp_10ms.txt`): $\text{m}$

---

## 3. Scaling Factors and Usage in Paper

1. **Normalized Baseline**:
   - Both acceleration records in this repository are normalized to a peak ground acceleration (PGA) of **$1.0\text{ m/s}^2$**.
2. **Dynamic Analysis & RTHS Testing**:
   - In the OpenSees SSI time-history analysis (`main.tcl`) and RTHS benchmark tests, an amplitude scaling factor of **$1.47$** was applied:
     $$\text{PGA}_{\text{input}} = 1.0\text{ m/s}^2 \times 1.47 = 1.47\text{ m/s}^2 \approx 0.15\text{ g}$$
3. **Paper Illustration / Visualization**:
   - For time-history comparison plots (e.g., ground motion figures), the records were windowed/padded (5 s leading zeros) and scaled to a nominal target peak of **$0.10\text{ g}$** ($0.98\text{ m/s}^2$) for clean visual presentation.
