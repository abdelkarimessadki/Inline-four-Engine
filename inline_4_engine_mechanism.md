# Inline‑4 Engine Mechanism & CATIA V5 Project

> Author: Abdelkarim Essadki  
> CAD Platform: **CATIA V5**  
> Assembly: **ENGINE ASSEMBLY.CATProduct** (Inline‑4)

---

## 1) What this project is
This repository documents a complete **Inline‑4 (I4) engine core assembly** modeled in **CATIA V5**. It includes the engine’s structural block, rotating assembly, and one piston‑connecting‑rod subassembly replicated ×4. The focus is on **clean product structure, motion correctness, and manufacturable parts** rather than full ancillaries (head, timing, intake, etc.).

**Delivered CATIA files** (as referenced in the screenshots):
- `ENGINE ASSEMBLY.CATProduct`  
- `ENGINE BLOCK.CATPart`  
- `CRANK SHAFT.CATPart`  
- `CONNECTING ROD SUB ASSEMBLY.CATProduct` (instanced ×4)  
  - `PISTON HEAD.CATPart`  
  - `PISTON PIN.CATPart`  
  - `CON ROD.CATPart`  
  - `ROD CAP.CATPart`

A high‑level **component tree** diagram is provided in `/docs/inline4_engine_tree.png` (and a wide/readable version `/docs/inline4_engine_tree_v3_spacing.png`).

---

## 2) How an Inline‑4 works (mechanism overview)
An Inline‑4 engine has **four cylinders in a straight line** sharing one **crankshaft**. Each cylinder has a **piston** that reciprocates; a **connecting rod** converts this **linear motion** into **rotational motion** at the crankshaft.

### 2.1 Motion chain
1. **Combustion pressure** pushes the **piston crown** downward.  
2. Load flows through **pin bosses → piston pin (gudgeon pin)**.  
3. **Con‑rod small end → con‑rod shank → big end/bearing shells**.  
4. **Crank rod journal → crank throw → main journals/bearings**.  
5. Output torque leaves via the **flywheel**/front accessories.

### 2.2 Kinematics
- Piston displacement (approx.):  \( x(\theta) = r\cos\theta + \frac{r^2}{4l}\cos(2\theta) + \dots \)  
  where \( r \) = crank radius, \( l \) = con‑rod length, \( \theta \) = crank angle.  
- Velocity and acceleration contain **2nd‑order components** → relevant for **balancing**.

### 2.3 Firing order & balance
- A common I4 firing order is **1‑3‑4‑2** (evenly spaced 180° crank intervals).  
- Primary reciprocating forces cancel, but **secondary** do not → modern I4s often use **balance shafts** (not modeled here).

### 2.4 Lubrication & cooling (core concepts)
- **Lubrication:** Oil is pumped to **main bearings**, cross‑drilled through the crank to **rod bearings**, then to **pins** (via splash/jet/small‑end gallery).  
- **Cooling:** **Water jackets** around bores remove heat; piston crown cooling by conduction through rings/skirt and—in high‑load engines—**oil jets**.

---

## 3) Project structure & CAD choices
### 3.1 Product tree
```
ENGINE ASSEMBLY.CATProduct
├─ ENGINE BLOCK.CATPart
├─ CRANK SHAFT.CATPart
└─ CONNECTING ROD SUB ASSEMBLY.CATProduct ×4
   ├─ PISTON HEAD.CATPart
   ├─ PISTON PIN.CATPart
   ├─ CON ROD.CATPart
   └─ ROD CAP.CATPart
```

### 3.2 Modeling intent
- **Block**: datuming for bore centerlines, deck plane, and main bearing line; water‑jacket volume simplified.  
- **Crank**: main/rod journal diameters, fillet radii, counterweights; oil holes represented for realism.  
- **Con‑rod**: I‑beam section, **fracture‑split big end** (cap); small‑end bushing present.  
- **Piston**: crown, ring lands/grooves, skirt; pin bosses with correct wall thickness.  
- **Pin**: hollow to reduce mass; case‑hardening assumed.

### 3.3 Constraints (assembly)
- **Crankshaft**: constrained to block main axis (coaxial) with axial float suppressed for kinematic studies.  
- **Rod big end**: concentric with rod journal; cap aligned via split faces.  
- **Piston‑pin**: concentric with small end & piston bosses; piston axis parallel to bore axis.  
- **Linear stroke** controlled by crank rotation (Angle) → piston translation along bore (Distance).

### 3.4 Kinematic simulation in CATIA V5
- Use **DMU Kinematics** or mechanism constraints:  
  - Revolute joint: crankshaft about block main axis.  
  - Revolute joint: each rod about its journal.  
  - Cylindrical/Prismatic: piston along bore axis.  
- Drive the **crank angle**; verify **top dead center (TDC)** and **bottom dead center (BDC)** positions and clearances.

---

## 4) Design notes by part
- **ENGINE BLOCK**: Bore diameter & spacing define displacement; deck flatness and main‑saddle alignment are critical.  
- **CRANK SHAFT**: Balance with counterweights; check rod‑to‑block clearance at BDC; add damper at front (not modeled).  
- **CONNECTING ROD SUB‑ASSEMBLY (×4)**:  
  - **CON ROD + ROD CAP**: big‑end bearing crush/roundness assumed; rod bolts not modeled but implied.  
  - **PISTON PIN**: full‑floating design (circlips) assumed.  
  - **PISTON HEAD**: ring grooves modeled; orientation mark to front recommended.

---

## 5) How to open & view (CATIA V5)
1. Open `ENGINE ASSEMBLY.CATProduct`.  
2. Set the **Product Structure** tree visible.  
3. Activate **Constraints**; update if CATIA prompts.  
4. (Optional) In **DMU Kinematics**, define a command to drive **crank angle** and observe piston motion.

### Exporting views
- Use **Drafting** to generate standard views of each part.  
- For visuals, export images from **Rendering** or **Capture**.  
- Diagrams are in `/docs` (PNG & PDF); you can replace them with new captures.

---

## 6) Bill of Materials (BOM – simplified)
| Item | Part | Qty |
|---:|---|---|
| 1 | ENGINE BLOCK.CATPart | 1 |
| 2 | CRANK SHAFT.CATPart | 1 |
| 3 | CONNECTING ROD SUB ASSEMBLY.CATProduct | 4 |
| 3.1 | PISTON HEAD.CATPart | 4 |
| 3.2 | PISTON PIN.CATPart | 4 |
| 3.3 | CON ROD.CATPart | 4 |
| 3.4 | ROD CAP.CATPart | 4 |

> Note: Fasteners, bearings, rings, gaskets, and ancillaries are out of scope for this iteration.
