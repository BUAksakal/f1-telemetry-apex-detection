# 🏎️ F1 Apex Analyzer — Neon Track Edition

This project uses real Formula 1 telemetry to **automatically detect true apex points**, determine **the fastest driver for each corner**, and visualize everything on a **stylized neon circuit map**.  
Each apex is colored based on the driver’s **official team color**, and a 🏁 **checkered flag** marks the start/finish line.

Built with **Fast-F1**, **SciPy**, **NumPy** and **Plotly**.

---

## 🚀 Features

- **Automatic apex detection** from telemetry  
- **Curvature + speed derivative apex algorithm**  
- **Fastest driver per corner** (Mode B)  
- **Team-color apex markers**  
- **Neon-styled circuit visualization**  
- **Closed spline track (no gaps)**  
- 🏁 **Start/Finish checkered flag**  
- Hover tooltips showing driver + apex speed  

---

# 📊 Apex Detection — How It Works

Apex is determined mathematically using **two signals**:

---

### **1️⃣ Speed Minimum (Dynamic Apex)**

A car typically reaches minimum velocity at the apex:

$begin:math:display$
\\text\{ApexCandidate\} \= \\operatorname\*\{arg\\\,min\}\(V\(t\)\)
$end:math:display$

where:

- $begin:math:text$ V\(t\) $end:math:text$ = speed  
- $begin:math:text$ t $end:math:text$ = telemetry timestamp  

We also consider the first derivative:

$begin:math:display$
\\frac\{dV\}\{dt\}
$end:math:display$

to validate the braking → apex → acceleration pattern.

---

### **2️⃣ Curvature (Geometric Apex)**

Using GPS coordinates $begin:math:text$ x\(t\)\, y\(t\) $end:math:text$:

$begin:math:display$
\\kappa \= 
\\frac\{\|x\' y\'\' \- y\' x\'\'\|\}
\{\\left\(\(x\'\)\^\{2\} \+ \(y\'\)\^\{2\}\\right\)\^\{3\/2\}\}
$end:math:display$

High curvature = tight corner.  
Curvature peaks identify **corner intensity**.

$begin:math:display$
\\text\{CornerPeak\} \= \\operatorname\*\{arg\\\,max\}\(\\kappa\)
$end:math:display$

---

### ✔ Final Apex Decision

A point is accepted as the apex if:

$begin:math:display$
\\text\{Apex\} \= 
\(\\text\{Speed Min\}\) 
\\\;\\cap\\\; 
\(\\text\{Curvature Peak\}\)
$end:math:display$

within a small neighborhood:

$begin:math:display$
\|i\_\{\\text\{speed\}\} \- i\_\{\\text\{curv\}\}\| \< 25
$end:math:display$

This produces **accurate apex locations** for every driver.

---

# 🏆 Fastest Driver per Corner (Mode B)

For each detected apex:

$begin:math:display$
\\text\{FastestDriver\}\(T\_k\)
\=
\\operatorname\*\{arg\\\,max\}\_\{d \\in Drivers\}
\\left\( V\_d\(T\_k\) \\right\)
$end:math:display$

Where:

- $begin:math:text$ T\_k $end:math:text$ = corner k  
- $begin:math:text$ V\_d\(T\_k\) $end:math:text$ = apex speed of driver d  

The map highlights each apex using **team colors**.

---

# 🎨 Visualization

- Smooth track generated with closed spline:

$begin:math:display$
\\text\{splev\}\(\\text\{linspace\}\(0\,1\,3000\)\)\, \\quad per\=True
$end:math:display$

- Neon glow effect  
- Apex markers sized + colored by team  
- Hover tooltip shows:

```
Corner • Driver • Apex Speed
```

- 🏁 Start/Finish line marked with a checkered flag emoji

---

# 📦 Installation

```bash
pip install fastf1 plotly scipy numpy pandas
```

Enable cache:

```python
import fastf1
fastf1.Cache.enable_cache("f1cache")
```

---

# ▶️ Usage

```bash
python apex_analyzer.py
```

---

# 📁 Repository Structure

```
.
├── apex_analyzer.py
├── f1cache/
├── README.md
└── outputs/
    └── neon_apex_map.png
```

---

# 🖼️ Output
<img width="1500" height="820" alt="newplot" src="https://github.com/user-attachments/assets/f83b6ce5-d099-43d4-91bd-b6d68405fbc7" />
---

# 🔧 Technologies

- **Fast-F1** — session & telemetry data  
- **SciPy** — curvature & spline  
- **Plotly** — neon visualization  
- **NumPy / Pandas** — data processing  

---

# ⭐ About

Developed by **Berke Uğur Aksakal**.  
A combination of motorsport passion and AI engineering.
