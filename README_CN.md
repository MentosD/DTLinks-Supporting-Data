# DTLinks 论文支撑数据：地震动时程记录说明 (README_CN)

本数据仓库提供了论文中用于实时混合试验（RTHS）及 OpenSees 土-结构相互作用（SSI）数值仿真的地震动输入数据（加速度和位移时程）。

---

## 1. 数据文件清单及说明

| 文件名 | 地震波名称 / 事件 | 数据类型 | 采样步长 ($\Delta t$) | 数据点数 | 总持时 | 峰值数值 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`Case_I_ElCentro_Accel_10ms.txt`** | El Centro 波 (1940 Imperial Valley 地震, NS分量) | 地面加速度 | 0.010 s (10 ms) | 4418 | 44.17 s | 归一化为 $1.000\text{ m/s}^2$ |
| **`Case_I_ElCentro_Disp_10ms.txt`** | El Centro 波 (1940 Imperial Valley 地震, NS分量) | 地面位移 | 0.010 s (10 ms) | 4418 | 44.17 s | $0.0934\text{ m}$ |
| **`Case_II_Datui_Accel_10ms.txt`** | Datui 地震波 (Case II) | 地面加速度 | 0.010 s (10 ms) | 7501 | 75.00 s | 归一化为 $1.000\text{ m/s}^2$ |
| **`Case_II_Datui_Disp_10ms.txt`** | Datui 地震波 (Case II) | 地面位移 | 0.010 s (10 ms) | 7501 | 75.00 s | $0.1554\text{ m}$ |

---

## 2. 数据格式与单位规范

- **文件存储格式**：纯文本 ASCII (`.txt`)，单列浮点数值，无文件头（可直接通过 MATLAB `load()`、Python `numpy.loadtxt()` 或 OpenSees `timeSeries Path` 读取）。
- **采样频率**：$100\text{ Hz}$（采样间隔 $\Delta t = 0.010\text{ s} = 10\text{ ms}$）。
- **物理单位**：
  - 加速度文件 (`*_Accel_10ms.txt`)：$\text{m/s}^2$
  - 位移文件 (`*_Disp_10ms.txt`)：$\text{m}$

---

## 3. 调幅系数（Scaling Factor）及论文中使用情况

1. **基准归一化（Baseline Normalization）**：
   - 仓库中提供的加速度记录均已将峰值加速度（PGA）归一化调整为 **$1.0\text{ m/s}^2$**。
2. **时程分析与混合试验工况（RTHS Testing）**：
   - 在 OpenSees 动力时程分析（`main.tcl`）及 RTHS 实际加载试验中，加载的缩放系数 `factor` 统一设置为 **$1.47$**：
     $$\text{PGA}_{\text{input}} = 1.0\text{ m/s}^2 \times 1.47 = 1.47\text{ m/s}^2 \approx 0.15\text{ g}$$
3. **论文图表绘制说明**：
   - 在论文的地震波形示意图中，为保证两条地震波在排版上的直观对比，对时程进行了首尾补零（前 5 s 零值平滑过渡）并精准调幅至名义峰值 **$0.10\text{ g}$**（约 $0.98\text{ m/s}^2$）。
