# DTLinks 论文支撑数据：地震动时程与土-结构相互作用 (SSI) 模型说明

本仓库提供了论文中用于土-结构相互作用（SSI）数值仿真及实时混合试验（RTHS）的支撑数据与模型文件：
1. 标准化 $10\text{ ms}$ 地震动输入数据（包含加速度和位移时程）。
2. OpenSees 完整的非线性土-结构相互作用动力分析模型文件（`ssimodel.tcl`）。

---

## 1. 仓库文件清单

| 文件名 | 分类 | 说明 | 关键参数 / 特性 |
| :--- | :--- | :--- | :--- |
| **`ssimodel.tcl`** | OpenSees 模型 | 2D 土-结构相互作用 (SSI) 完整有限元模型 | 弹塑性纤维截面 + 多屈服面黏土本构 |
| **`Case_I_ElCentro_Accel_10ms.txt`** | 地震动输入 | El Centro 地面加速度 (1940 NS分量) | $\Delta t = 0.01\text{ s}$，4418点，归一化峰值 $1.000\text{ m/s}^2$ |
| **`Case_I_ElCentro_Disp_10ms.txt`** | 地震动输入 | El Centro 地面位移 (1940 NS分量) | $\Delta t = 0.01\text{ s}$，4418点，峰值 $0.0678\text{ m}$ |
| **`Case_II_Datui_Accel_10ms.txt`** | 地震动输入 | Datui 地面加速度 | $\Delta t = 0.01\text{ s}$，7501点，归一化峰值 $1.000\text{ m/s}^2$ |
| **`Case_II_Datui_Disp_10ms.txt`** | 地震动输入 | Datui 地面位移 | $\Delta t = 0.01\text{ s}$，7501点，峰值 $0.0779\text{ m}$ |

---

## 2. 土-结构相互作用 (SSI) 模型详解 (`ssimodel.tcl`)

`ssimodel.tcl` 包含了用于 OpenSees 的完整物理与数值模型定义：

### 2.1 上部结构与基础（显式材料非线性）
- **结构形式**：地上两层、两跨钢筋混凝土框架，底部设置地下单柱独立基础。
- **单元类型**：基于位移的非线性梁柱单元（`dispBeamColumn`），每个单元沿长度方向布置 4 个高斯积分点。
- **截面离散**：基于纤维截面（`section Fiber`），精细考虑轴力-弯矩强耦合以及截面沿深度的逐步屈服与塑性开展。
- **混凝土本构**：采用 `uniaxialMaterial Concrete01`（Kent-Scott-Park 滞回模型），考虑保护层未约束混凝土（$f'_c = 27.6\text{ MPa}$）与核心区约束混凝土（$f'_c = 34.5\text{ MPa}$）的抗压强化与软化段，受拉完全开裂。
- **钢筋本构**：采用 `uniaxialMaterial Hardening`，考虑明确的屈服强度（$f_y = 248.2\text{ MPa}$）、屈服平台及运动硬化（Kinematic Hardening）。
- **楼板假定**：各层楼面节点通过刚性隔膜约束（`rigidDiaphragm`）保证水平面内平动协调。

### 2.2 地基土体（多屈服面弹塑性本构）
- **土体几何尺寸**：水平宽 $32.4\text{ m}$（$-9.2\text{ m} \sim 23.2\text{ m}$），深度 $7.2\text{ m}$（$-7.2\text{ m} \sim 0\text{ m}$），平面外计算厚度 $0.60\text{ m}$。
- **单元类型**：二维 4 节点平面应变连续体单元（`element quadWithSensitivity`）。
- **土体材料本构**：采用 **`nDMaterial MultiYieldSurfaceClay`**（Prevost 多重屈服面弹塑性黏土本构）。该本构通过一系列嵌套屈服面，精确模拟动力循环荷载下地基土的**非线性剪切模量退化**、**滞回阻尼耗能**以及**不可逆残余塑性变形**。
- **土-基础相互作用界面**：基础节点与对应土体节点通过运动学耦合（`equalDOF`）实现变形传递。

---

## 3. 地震动数据与调幅使用说明

- **采样步长**：$\Delta t = 0.010\text{ s}$（$10\text{ ms}$，采样频率 $100\text{ Hz}$）。
- **文件格式**：单列纯文本 ASCII (`.txt`)，无文件头，易于直接导入。
- **物理量纲**：
  - 加速度文件：$\text{m/s}^2$
  - 位移文件：$\text{m}$
- **调幅使用说明**：
  - 基础文件已归一化至峰值 $1.0\text{ m/s}^2$。
  - 在论文动力分析与 RTHS 试验中，通过 `timeSeries Path` 乘上 `factor 1.47`，实际激振峰值为 $1.47\text{ m/s}^2 \approx 0.15\text{ g}$。
