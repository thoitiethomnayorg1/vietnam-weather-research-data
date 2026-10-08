# 🇻🇳 Vietnam Microclimate & Weather Dataset (34 Provinces)
### *Cơ sở Dữ liệu Vi khí hậu & Chỉ số Thời tiết Dân sinh 34 Tỉnh thành Việt Nam*

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23231033.svg)](https://doi.org/10.5281/zenodo.23231033)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Publisher](https://img.shields.io/badge/Publisher-ThoiTietHomNay.org-0284c7.svg)](https://thoitiethomnay.org/)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)

Official open research repository of **[ThoiTietHomNay.org](https://thoitiethomnay.org/)** — Comprehensive meteorological methodologies, biometeorological indexes (Heat Index, Dew Point, AQI PM2.5, UV Index), and standardized microclimate datasets across 34 administrative jurisdictions of Vietnam.

---

## 📌 Repository Contents

```
├── data/
│   ├── vietnam_34_provinces_microclimate_2026.csv   # Structured tabular dataset (CSV)
│   └── vietnam_34_provinces_microclimate_2026.json  # JSON schema dataset for developers
├── docs/
│   ├── Bao-Cao-Ky-Thuat-Vi-Khi-Hau-34-Tinh-Thanh-ThoiTietHomNay.pdf  # Full Whitepaper (7 pages)
│   └── Slide-Tom-Tat-Bao-Cao-Vi-Khi-Hau-ThoiTietHomNay-2026.pdf      # Executive Slide Deck (12 slides)
├── CITATION.cff                                    # GitHub citation metadata
├── LICENSE                                         # Creative Commons Attribution 4.0
└── README.md                                       # Documentation & usage guide
```

---

## 🔬 Mathematical Formulations

### 1. Apparent Temperature (Heat Index - HI)
Formulated via multi-variable Rothfusz regression (calibrated for tropical humid conditions $T \ge 27^\circ C$, $RH \ge 40\%$):

$$HI = -42.379 + 2.04901523 T + 10.14333127 RH - 0.22475541 T \cdot RH - 0.00683783 T^2 - 0.05481717 RH^2 + 0.00122874 T^2 RH + 0.00085282 T \cdot RH^2 - 0.00000199 T^2 RH^2$$

### 2. Dew Point Temperature ($T_d$) & Damp Weather Alert (Nồm Ẩm)
Calculated via Magnus-Tetens approximation ($a = 17.27, b = 237.7^\circ C$):

$$\gamma(T, RH) = rac{a \cdot T}{b + T} + \ln\left(rac{RH}{100}ight)$$

$$T_d = rac{b \cdot \gamma(T, RH)}{a - \gamma(T, RH)}$$

*Nồm ẩm Warning Gate:* Triggered when $RH \ge 85\%$ and $\Delta T = (T_{	ext{surface}} - T_d) \le 1.5^\circ C$.

### 3. Air Quality Index (AQI PM2.5)
Aligned with Vietnamese National Technical Regulation **QCVN 05:2023/BTNMT** and US EPA standards.

---

## 📊 Dataset Schema (`vietnam_34_provinces_microclimate_2026.csv`)

| Field Name | Type | Description |
| :--- | :---: | :--- |
| `code` | string | Unique province/city alpha code |
| `name` | string | Official Vietnamese province name |
| `region` | string | Ecological/climatic sub-region |
| `lat`, `lon` | float | Geographic coordinates (WGS84) |
| `avg_temp_summer_c` | float | Mean summer physical shelter temperature (°C) |
| `realfeel_max_c` | float | Maximum Heat Index / RealFeel apparent temperature (°C) |
| `humidity_winter_spring_pct`| float | Relative humidity mean during winter-spring (%) |
| `dew_point_avg_c` | float | Average dew point temperature (°C) |
| `nom_am_days_per_year` | integer | Number of high-humidity damp condensation days per year |
| `winter_aqi_pm25_avg` | float | Average morning PM2.5 AQI during winter thermal inversion |
| `uv_max` | float | Peak Summer Solar UV Index |
| `hub_url` | string | Canonical data portal URL on `ThoiTietHomNay.org` |

---

## 💻 Quick Start (Python)

```python
import pandas as pd

# Load dataset
url = "https://raw.githubusercontent.com/thoitiethomnay/vietnam-weather-research-data/main/data/vietnam_34_provinces_microclimate_2026.csv"
df = pd.read_csv("data/vietnam_34_provinces_microclimate_2026.csv")

# Filter urban heat island gap
df['heat_gap_c'] = df['realfeel_max_c'] - df['avg_temp_summer_c']
top_heat_gap = df.sort_values(by='heat_gap_c', ascending=False)[['name', 'region', 'heat_gap_c']]
print(top_heat_gap.head())
```

---

## 📖 Citation (APA 7th & BibTeX)

If you utilize this dataset, methodologies, or whitepaper in your academic publications or news reporting, please cite:

```bibtex
@techreport{thoitiethomnay2026microclimate,
  author      = {ThoiTietHomNay.org Data Science Council},
  title       = {Methodology for Microclimate Data Collection and Standardization Across 34 Provinces in Vietnam},
  institution = {ThoiTietHomNay Meteorological & Microclimate Research Center},
  year        = {2026},
  number      = {WTR-VN-2026-01},
  doi         = {10.5281/zenodo.23231033},
  url         = {https://thoitiethomnay.org/}
}
```

---

## 🌐 Official Portals & Contact

- **Primary Meteorological Portal:** [https://thoitiethomnay.org/](https://thoitiethomnay.org/)
- **Permanent Academic DOI:** [https://doi.org/10.5281/zenodo.23231033](https://doi.org/10.5281/zenodo.23231033)
- **LinkedIn Official Publication:** [Thời Tiết Hôm Nay Org on LinkedIn](https://www.linkedin.com/company/thoitiethomnayfanpage)
- **Google Sites Knowledge Hub:** [Thư viện Vi Khí Hậu Việt Nam](https://sites.google.com/view/thoitiethomnay-org/trang-chu)
