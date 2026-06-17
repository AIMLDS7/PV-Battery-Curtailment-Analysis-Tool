<div align="center">

# ⚡ 922 MWh Recovered.

### A grid-constrained Austrian PV site. A 4.3 MWh battery. One year of 15-minute data.

**This tool tells you exactly how much energy you're losing, why, and how much storage you'd need to fix it.**

<br>

```
  ☀️  PV Array (10.16 MW DC)
       │
       ▼
  ┌─────────────┐     Curtailed Energy     ┌──────────────┐
  │  9 MW Grid  │ ◄── ──── ──── ──── ──── │  BESS 4.3MWh │
  │    Limit    │                          │   / 2.16 MW  │
  └─────────────┘     Discharge to Grid   └──────────────┘
       │                   ▲                      │
       ▼                   └──────────────────────┘
  🔌  Grid Injection
```

<br>

[![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Plotly](https://img.shields.io/badge/Plotly-Interactive-3F4F75?style=flat-square&logo=plotly&logoColor=white)](https://plotly.com/)
[![Resolution](https://img.shields.io/badge/Resolution-15--minute-F5A623?style=flat-square)](https://github.com/AIMLDS7)
[![Strategies](https://img.shields.io/badge/BESS_Strategies-7-00D4C8?style=flat-square)](https://github.com/AIMLDS7)
[![Years](https://img.shields.io/badge/Validated-2024_%26_2025-2ECC71?style=flat-square)](https://github.com/AIMLDS7)
[![Status](https://img.shields.io/badge/Status-Production_Ready-brightgreen?style=flat-square)](https://github.com/AIMLDS7)

</div>

---

## 🎯 The Problem, Quantified

Most PV+BESS feasibility studies make one of three mistakes: simplified battery physics, single-year analysis, or testing only 1–2 dispatch strategies. The result is sizing recommendations that don't hold up in practice.

This tool makes none of those compromises.

| Site Parameter | Value |
|---|---|
| 📍 Location | Austria |
| ☀️ PV Capacity | **10.16 MW DC** |
| 🔌 Grid Injection Limit | **9 MW** |
| 🔋 Battery | **4.3 MWh / 2.16 MW** |
| ⏱️ Resolution | **15-minute** (35,040 intervals/year) |
| 📅 Analysis Period | **Full calendar years 2024 & 2025** |
| 🧪 Strategies Tested | **7 physically-correct BESS algorithms** |

---

## 📈 Headline Results

> *Best-performing strategy vs. no-storage baseline. Consistent across both years.*

| Year | Baseline Curtailment | With 4.3 MWh BESS | **Energy Recovered** | Reduction | Battery Cycles |
|------|---------------------|-------------------|----------------------|-----------|----------------|
| **2024** | 6,110 MWh (14.1%) | 5,188 MWh (12.0%) | **+922 MWh** | 2.1 pp | ~196 |
| **2025** | 5,924 MWh (14.1%) | 5,016 MWh (12.0%) | **+908 MWh** | 2.1 pp | ~193 |

**A 4.3 MWh battery recovers ~15% of the energy lost to the grid limit — with only ~195 full cycles per year.**

The consistency across 2024 and 2025 is not a coincidence. It validates the model and the site's solar regime simultaneously.

---

## 🧠 What Makes This Analysis Rigorous

### ⚙️ Physically Correct Battery Model

Most academic implementations allow the battery to discharge freely during curtailment windows — which is physically impossible. This model enforces strict energy causality:

```
IF  Net_Injection(t) >= Grid_Limit:
    BESS can CHARGE from excess                 ✅
    BESS cannot DISCHARGE (grid already at cap)  ✅

IF  Net_Injection(t) < Grid_Limit:
    BESS can DISCHARGE to boost injection        ✅
    BESS cannot create energy from nowhere       ✅
```

**Round-trip efficiency**: 91.3% applied bidirectionally (95.5% per direction)  
**Continuous curtailment windows**: correctly handled — summer multi-hour saturation periods are modeled accurately

---

## 🔬 Four-Scenario Framework

```
          S1                    S2                    S3                    S4
   ┌─────────────┐      ┌─────────────┐      ┌─────────────┐      ┌─────────────┐
   │ PV + 9MW    │      │ PV Only     │      │ PV + BESS   │      │ PV + BESS   │
   │ Grid Limit  │      │ No Limit    │      │ + 9MW Limit │      │ No Limit    │
   │             │      │             │      │             │      │             │
   │ Baseline    │      │ Theoretical │      │ ★ Primary   │      │ Arbitrage   │
   │ (Reality)   │      │ Maximum     │      │   Value     │      │   Only Mode │
   └─────────────┘      └─────────────┘      └─────────────┘      └─────────────┘
```

**S2 − S1** = Maximum possible curtailment (the ceiling)  
**S3 − S1** = Actual value delivered by storage  
**S3 / S2** = Storage effectiveness ratio  
**S4** = Upper bound for battery arbitrage revenue (no grid constraint)

---

## ⚡ Seven BESS Dispatch Strategies

All strategies share the **same charging logic** (charge only from curtailed excess). They differ exclusively in *when* and *how aggressively* they discharge. This isolates discharge timing as the variable under test.

```
Performance Ranking (consistent across 2024 & 2025)
─────────────────────────────────────────────────────

  🥇 TIER 1 — Feed-in Maximizers
  ├── Peak Shaving          ████████████████████  Best overall
  ├── Aggressive Discharge  ███████████████████░
  └── Feed-in Maximization  ██████████████████░░

  🥈 TIER 2 — Time-Based
  ├── Night Discharge        ███████████████░░░░░
  └── Evening Boost          ██████████████░░░░░░

  🥉 TIER 3 — Rate-Proportional
  └── Proportional Charge    █████████████░░░░░░░

  ❌ UNDERPERFORMER
  └── Smart Buffer           ████████████░░░░░░░░  15% SOC reserve kills results
```

> **Strategy choice matters.** The gap between best and worst is **hundreds of MWh per year**.

---

## 📊 Visual Gallery

### Strategy Performance Ranking

![Strategy Ranking](images/strategyranking.jpg)

---

### Capacity Sensitivity — Where Diminishing Returns Begin

**2025**
![Capacity Sensitivity 2025](images/capacity_sensitivity_2025.jpg)

**2024**
![Capacity Sensitivity 2024](images/capacity_sensitivity_2024.jpg)

---

### Full Energy Balance (All 4 Scenarios)

**2025**
![Complete Energy Balance 2025](images/complete_2025.jpg)

**2024**
![Complete Energy Balance 2024](images/complete_2024.jpg)

---

### Four-Scenario Curtailment Comparison

**2025**
![4-Scenario 2025](images/4_Scenario_Curtailment_Comparison_2025.jpg)

**2024**
![4-Scenario 2024](images/4_Scenario_Curtailment_Comparison_2024.jpg)

---

### Monthly Breakdown & Strategy Comparison

| 2025 | 2024 |
|------|------|
| ![Monthly 2025](images/Monthly_2025.jpg) | ![Monthly 2024](images/Monthly_2024.jpg) |
| ![Strategy 2025](images/stretagy_2025.jpg) | ![Strategy 2024](images/stretagy_2024.jpg) |

---

### Production-Ready GUI

![GUI](images/PV-Battery_Analysis_Tool.jpg)

---

## 📐 Battery Sizing Intelligence

The tool runs automated capacity sweeps and interpolates to find the battery size needed to hit any curtailment reduction target. Cross-year averages smooth out solar variability.

| Target Curtailment Reduction | Battery Required | Power Required | Scale vs Current |
|------------------------------|-----------------|----------------|-----------------|
| **25%** (baseline case) | ~4.3 MWh | ~2.2 MW | 1× (current system) |
| **50%** | ~18.2 MWh | ~9.1 MW | 4.2× |
| **75%** | ~41 MWh | ~20.5 MW | 9.5× |
| **90%+** | Not achievable (≤80 MWh) | — | Strong diminishing returns |

> **Key insight**: After ~20–25 MWh, each additional MWh of storage recovers less and less energy. The grid limit creates an absolute ceiling that no amount of storage can overcome without upgrading the grid connection.

```
  Curtailment
  Reduction
  100% │                                        ╭─────────
       │                                   ╭────╯
   75% │                              ╭────╯
       │                         ╭────╯
   50% │                    ╭────╯
       │               ╭────╯
   25% │ ★────────────╯
       │ Current
    0% └──────────────────────────────────────────────────
        0    5    10    20    30    40    50+   MWh
                                              Battery Size
```

---

## 🚀 Quick Start

```bash
# Install dependencies
pip install pandas numpy plotly python-docx kaleido openpyxl

# Run the tool
python pv_battery_Curtailment_V28.py
```

Update the data file paths at the top of the script for your PV and load data.

**Recommended workflow:**

```
Step 1 → Run 4-SCENARIO COMPARISON
         Select multiple strategies to compare
                  │
                  ▼
Step 2 → Review interactive Plotly dashboards
         + auto-generated DOCX technical report
                  │
                  ▼
Step 3 → Run CAPACITY SENSITIVITY
         Get interpolated sizing recommendations
         for your curtailment reduction targets
```

---

## 📦 Generated Outputs

| File | Contents |
|------|----------|
| `4Scenario_YYYY.xlsx` | Full energy balance, all scenarios, both years |
| `StrategyComparison_YYYY.xlsx` | All 7 strategies ranked side-by-side |
| `PV_BESS_Curtailment_Report_YYYY.docx` | Complete technical report with embedded figures & tables |
| Plotly `.html` | Interactive dashboards with zoom, range selectors, hover data |

---

## 📁 Project Structure

```
pv-battery-curtailment/
├── README.md
├── requirements.txt
├── pv_battery_Curtailment_V28.py          ← main analysis script
│
├── images/                                ← charts & screenshots
│   ├── capacity_sensitivity_2024.jpg
│   ├── capacity_sensitivity_2025.jpg
│   ├── strategyranking.jpg
│   ├── complete_2024.jpg
│   ├── complete_2025.jpg
│   ├── 4_Scenario_Curtailment_Comparison_2024.jpg
│   ├── 4_Scenario_Curtailment_Comparison_2025.jpg
│   ├── Monthly_2024.jpg
│   ├── Monthly_2025.jpg
│   ├── stretagy_2024.jpg
│   ├── stretagy_2025.jpg
│   └── PV-Battery_Analysis_Tool.jpg
│
├── data/                                  ← input data (not committed)
│   ├── pvsyst_pv1.csv
│   ├── pvsyst_pv2.csv
│   └── Lastprofil_2024_2025.xlsx
│
└── outputs/                               ← generated reports (gitignored)
```

---

## ⚠️ Limitations (Full Transparency)

This tool is production-quality for techno-economic pre-feasibility. For investment-grade analysis, note:

- **No battery degradation**: calendar aging and cycle-life degradation not modeled
- **No temperature effects**: round-trip efficiency is fixed, not temperature-dependent
- **Site-specific**: results are tied to this site's load profile and Austrian solar resource
- **Multi-cycling constraint**: long summer curtailment windows naturally limit annual cycle count — modeled correctly, but no active cycle optimization
- **>75% reduction**: not achievable with any commercially sensible battery at this site without a grid connection upgrade

---

## 🏆 How This Compares to Academic Benchmarks

| Feature | Typical Academic Study | This Tool |
|---------|----------------------|-----------|
| Battery physics | Simplified / idealized | ✅ Strict causality enforced |
| Resolution | Hourly | ✅ 15-minute |
| Analysis period | 1 year (often TMY) | ✅ 2 real calendar years |
| Strategies tested | 1–3 | ✅ 7 production-grade strategies |
| Sensitivity analysis | Rare | ✅ Full capacity sweep with interpolation |
| Output formats | Figures only | ✅ Interactive dashboards + Excel + DOCX |
| Cross-year validation | Almost never | ✅ 2024 & 2025 independently validated |
| Strategy impact quantified | Rarely | ✅ Hundreds of MWh/year gap demonstrated |

---

## 💻 Code Availability

The full source code is **not publicly released**.

It **can be shared on request** for:
- Academic collaboration or research partnerships
- Commercial licensing discussions
- Independent verification of methodology

Please reach out via **[GitHub](https://github.com/AIMLDS7)** or email.

---

<div align="center">

*Real 15-minute Austrian grid data · 70,080 intervals analyzed · June 2026*

**[GitHub: AIMLDS7](https://github.com/AIMLDS7)**

</div>
