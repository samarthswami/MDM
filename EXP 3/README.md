# Experiment 3: Demonstration of Frequency Reuse Concept in Cellular Systems Using Python

[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)

---

---

## 📌 Experiment Metadata
- **Experiment No:** 03
- **Title:** Demonstration of Frequency Reuse Concept in Cellular Systems Using Python
- **Aim:** To understand and demonstrate the concept of frequency reuse in cellular mobile radio networks, calculate cluster sizes, co-channel reuse distance, and simulate a 7-cell frequency reuse cluster layout ($N=7$) across multiple adjacent clusters using Python.

---

## 🛠️ Requirements
- **Hardware:** Desktop / Laptop
- **Software:** Python 3.x (Google Colab / Jupyter Notebook / VS Code)
- **Libraries:** `numpy`, `matplotlib` (specifically `matplotlib.patches.RegularPolygon`)

---

## 📖 Core Theory & Principles

### 1. The Cellular Concept & Why Frequency Reuse?
In early mobile telephone systems, a single high-power transmitter situated on a mountain or tall tower covered an entire metropolitan area. While coverage was large, the system supported only a few dozen simultaneous calls because the available radio spectrum was strictly limited.

To overcome capacity limits, Bell Labs developed the **Cellular Concept**:
- Instead of using one high-power transmitter, a large geographical service area is divided into many small coverage zones called **cells**, each served by a lower-power Base Station (BS).
- Rather than assigning unique frequency channels to every cell, the total available radio spectrum is split into a discrete set of channel groups (a **cluster** of size $N$).
- The same group of frequency channels is **reused** in multiple other cells that are separated from each other by a sufficient physical distance so that Co-Channel Interference (CCI) remains within tolerable limits.

```
       Cluster 1                     Cluster 2
   ┌───┬───┬───┬───┐             ┌───┬───┬───┬───┐
   │ A │ B │ C │ D │             │ A │ B │ C │ D │
   ├───┼───┼───┼───┤             ├───┼───┼───┼───┤
   │ E │ F │ G │...│             │ E │ F │ G │...│
   └───┴───┴───┴───┘             └───┴───┴───┴───┘
         ▲                             ▲
         │                             │
         └──── Co-Channel Cells (A) ───┘
           Separated by Distance D
```

---

### 2. Geometry: Why Hexagonal Cells?
While real radio coverage contours are irregular circular blobs (due to terrain and shadowing), network design requires geometric tessellation (covering a 2D plane completely without overlapping and without empty gaps):
- Only three regular polygons can tessellate a 2D plane: **Equilateral Triangles**, **Squares**, and **Regular Hexagons**.
- The **hexagon** has the highest geometric area for a given perimeter, provides the closest approximation to a circle, and ensures equal distances to all six neighboring cells.

For a regular hexagon of radius $R$ (distance from center to any vertex):
- Center-to-flat side distance: $d = \frac{\sqrt{3}}{2} R$
- Horizontal distance between neighboring cell centers: $dx = 1.5 R$
- Vertical distance between neighboring cell centers: $dy = \sqrt{3} R$

---

### 3. Mathematical Foundations of Cellular Frequency Planning

#### A. Cluster Size ($N$)
To tessellate hexagonal cells without gaps, the cluster size $N$ must satisfy the relation:
$$N = i^2 + i \cdot j + j^2$$
Where $i$ and $j$ are non-negative integers representing shift parameters along the hexagonal coordinate axes.

Common allowed cluster sizes:
- $i=1, j=1 \implies N = 1^2 + (1)(1) + 1^2 = \mathbf{3}$
- $i=2, j=0 \implies N = 2^2 + (2)(0) + 0^2 = \mathbf{4}$
- $i=2, j=1 \implies N = 2^2 + (2)(1) + 1^2 = \mathbf{7}$ (Most widely used standard)
- $i=3, j=0 \implies N = 3^2 + (3)(0) + 0^2 = \mathbf{9}$
- $i=2, j=2 \implies N = 2^2 + (2)(2) + 2^2 = \mathbf{12}$

#### B. Co-Channel Reuse Distance ($D$)
The minimum physical center-to-center distance between two cells that use the exact same frequency channel group:
$$D = R \times \sqrt{3N}$$

#### C. Co-Channel Reuse Ratio ($Q$)
A dimensionless measure of how far co-channel cells are separated relative to cell radius:
$$Q = \frac{D}{R} = \sqrt{3N}$$
- For $N = 7$: $Q = \sqrt{3 \times 7} = \sqrt{21} \approx 4.58$
- A larger $Q$ or $N$ gives greater separation between co-channel cells, drastically reducing interference, but lowers system capacity per unit area.

#### D. Signal-to-Interference Ratio ($S/I$)
Considering the first tier of 6 co-channel interfering cells:
$$\frac{S}{I} = \frac{R^{-n}}{\sum_{k=1}^{6} D_k^{-n}} \approx \frac{(\sqrt{3N})^n}{6}$$
Where $n$ is the path loss exponent ($n \approx 3.5 - 4$ in urban propagation). For satisfactory voice quality in AMPS/GSM, an $S/I \ge 18 \text{ dB}$ is required, which dictates choosing **$N = 7$**.

---

## 💻 Google Colab Code (Copy-Paste Ready)

Copy and run the code below in Google Colab to visualize a 7-Cell Frequency Reuse Cluster ($N=7$) across 3 adjacent clusters:

```python
# ==============================================================================
# Experiment 3: Frequency Reuse in Cellular Mobile Networks (N=7 Cluster)
# Run this cell directly in Google Colab or Jupyter Notebook
# ==============================================================================

import numpy as np
import matplotlib.pyplot as plt
from matplotlib.patches import RegularPolygon

# ------------------------------------------------------------------------------
# 1. Hexagon Geometry Setup
# ------------------------------------------------------------------------------
R = 1.0                    # Hexagon radius (center to vertex)
dx = 1.5 * R               # Horizontal center-to-center distance
dy = np.sqrt(3) * R        # Vertical spacing

# Distinct pastel/vibrant colors for the 7 frequency channels (A through G)
cell_colors = {
    'A': '#FF6B6B',   # Coral Red
    'B': '#4D96FF',   # Royal Blue
    'C': '#6BCB77',   # Bright Green
    'D': '#FFD93D',   # Warm Yellow
    'E': '#9B51E0',   # Purple
    'F': '#00F5D4',   # Turquoise Cyan
    'G': '#FF9F45'    # Orange
}

# ------------------------------------------------------------------------------
# 2. Local coordinates for 7 cells forming ONE cluster (N=7)
# ------------------------------------------------------------------------------
cluster = [
    (0,        dy,     'B'),   # Top
    (-dx,      dy/2,   'G'),   # Upper-left
    ( dx,      dy/2,   'C'),   # Upper-right
    (0,        0,      'A'),   # Center
    (-dx,     -dy/2,   'F'),   # Lower-left
    ( dx,     -dy/2,   'D'),   # Lower-right
    (0,       -dy,     'E')    # Bottom
]

# ------------------------------------------------------------------------------
# 3. Offsets to place 3 adjacent clusters (showing seamless frequency reuse)
# ------------------------------------------------------------------------------
cluster_offsets = [
    (0, 0),                  # Center cluster
    (-3 * R, -2 * dy),       # Southwest cluster
    (1.5 * R, -2.5 * dy)     # Southeast cluster
]

# Assemble all cells across all clusters
all_cells = []
for ox, oy in cluster_offsets:
    for cx, cy, label in cluster:
        all_cells.append((cx + ox, cy + oy, label))

# ------------------------------------------------------------------------------
# 4. Plotting the Hexagonal Lattice
# ------------------------------------------------------------------------------
fig, ax = plt.subplots(figsize=(10, 8))

for x, y, label in all_cells:
    # orientation=np.radians(30) ensures flat-top / vertical vertices
    hex_patch = RegularPolygon(
        (x, y),
        numVertices=6,
        radius=R,
        orientation=np.radians(30),
        facecolor=cell_colors[label],
        edgecolor='black',
        linewidth=1.8,
        alpha=0.75
    )
    ax.add_patch(hex_patch)
    
    # Text label for frequency group inside each cell
    ax.text(
        x, y, label,
        ha='center', va='center',
        fontsize=13, fontweight='bold',
        color='black'
    )

# Formatting figure
ax.set_aspect('equal')
ax.set_xlim(-6.5, 6.0)
ax.set_ylim(-8.0, 3.5)
ax.axis('off')

# Title and Legend
plt.title("Cellular Frequency Reuse Concept (Cluster Size N = 7)\n"
          "Notice how channel 'A' is repeated across clusters without adjacent overlap!", 
          fontsize=13, fontweight='bold', pad=15)

# Build custom legend for Frequency Groups A through G
legend_handles = [
    plt.Line2D([0], [0], marker='s', color='w', label=f"Group {k}",
               markerfacecolor=cell_colors[k], markersize=12, markeredgecolor='black')
    for k in sorted(cell_colors.keys())
]
ax.legend(handles=legend_handles, loc='upper right', frameon=True, fontsize=10, title="Frequency Channels")

plt.tight_layout()
plt.show()
```

---

## 🎯 Viva Questions & Answers (Experiment 3)

### Q1. What is the concept of frequency reuse in cellular networks?
**Answer:** Frequency reuse is the technique of allocating the same group of radio frequencies to multiple cell base stations, provided those cells are separated by a sufficient physical distance ($D$) so that co-channel interference remains below an acceptable threshold.

### Q2. Why is frequency reuse necessary in mobile communications?
**Answer:** Because the radio frequency spectrum is finite, scarce, and costly. Without frequency reuse, an entire city could only support as many calls as the total number of allocated channels (e.g., a few hundred). Frequency reuse multiplies system capacity to support millions of concurrent users.

### Q3. Why are cells geometrically modeled as regular hexagons?
**Answer:** A hexagon is one of only three regular polygons (triangle, square, hexagon) that can tessellate a 2D plane with zero gaps and zero overlap. Among these three, the hexagon has the greatest area for a given perimeter, best approximates a circular radio antenna radiation pattern, and provides equal distance to all six adjacent neighbors.

### Q4. What is the mathematical formula for allowed cluster size $N$?
**Answer:**
$$N = i^2 + i \cdot j + j^2$$
Where $i$ and $j$ are non-negative integers ($i, j \ge 0$). Valid values include $N = 1, 3, 4, 7, 9, 12, \dots$

### Q5. What is the co-channel reuse distance ($D$)?
**Answer:** Co-channel reuse distance $D$ is the minimum center-to-center distance between two cells that use the exact same frequency channel set. It is given by:
$$D = R \times \sqrt{3N}$$
Where $R$ is the cell radius.

### Q6. What is the Co-Channel Reuse Ratio ($Q$) for an $N=7$ cluster?
**Answer:**
$$Q = \frac{D}{R} = \sqrt{3N} = \sqrt{3 \times 7} = \sqrt{21} \approx 4.58$$

### Q7. What is Co-Channel Interference (CCI)?
**Answer:** CCI is radio interference caused by other base stations or mobile handsets that operate on the **exact same frequency channel** within nearby clusters. Unlike background thermal noise, CCI cannot be overcome by simply increasing transmitter power, because boosting power also boosts the interference by the same proportion.

### Q8. What is the trade-off in selecting cluster size $N$?
**Answer:**
- **Small Cluster Size (e.g., $N=3$ or $4$):** More channels per cell $\implies$ higher system capacity. However, co-channel cells are closer together ($D$ is smaller) $\implies$ higher Co-Channel Interference ($S/I$ drops).
- **Large Cluster Size (e.g., $N=12$):** Co-channel cells are farther apart $\implies$ very low interference ($S/I$ is high). However, fewer channels per cell $\implies$ lower overall system subscriber capacity.
- **$N=7$** is the universal compromise providing $S/I \ge 18 \text{ dB}$ for high voice quality.

### Q9. What are Co-Channel Cells?
**Answer:** Co-channel cells are cells across different geographical clusters that share the exact same frequency channel assignment (e.g., all cells marked with letter 'A' across the clusters).

### Q10. What is Cell Splitting and Cell Sectoring?
**Answer:**
- **Cell Splitting:** Subdividing congested cells into smaller microcells with reduced antenna heights and transmit powers, increasing capacity in dense urban centers.
- **Cell Sectoring:** Replacing omnidirectional base station antennas with directional antennas (e.g., three $120^\circ$ sectors or six $60^\circ$ sectors) to reduce the number of co-channel interferers from 6 down to 2 or 1.


---
[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)
