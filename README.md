# Kinematic and Dynamic Analysis of a Custom 4-DOF Robotic Manipulator

This repository contains the computational modeling, kinematic validation, and rigid body dynamic analysis of a custom 4-Degree-of-Freedom (4-DOF) robotic manipulator. Developed using **MATLAB and Simscape Multibody** as part of the **Bharat Space Education Research Centre (BSERC) Def-Space Summer Internship Program (19th June – 30th July 2026)**.

---

## 👥 Authors
* **G Ananth Srivasthava** (Enrollment: `BSERC-10032`) — *IIITDM Kurnool*
* **Adithi R** (Enrollment: `BSERC-07142`) — *JSS Academy of Technical Education, Bengaluru*

---


##  System Architecture & Specifications
The manipulator features a serialized kinematic chain built with parameterized **Cylindrical Solid** blocks and connected via four **Revolute Joints** (Base, Shoulder, Elbow, and Wrist). Orthogonal orientation frames were set using Rigid Transform blocks ($+X$ rotations at $90^\circ$ and $-90^\circ$).

### Standard SI Dimensions:
* **Base Link:** Diameter = $0.10\text{ m}$, Length ($L_{base}$) = $0.10\text{ m}$.
* **Upper Arm (Shoulder-Elbow):** Length ($L_{upper}$) = $0.20\text{ m}$, Diameter = $0.04\text{ m}$.
* **Lower Arm (Elbow-Wrist):** Length ($L_{lower}$) = $0.10\text{ m}$, Diameter = $0.03\text{ m}$.
* **End Effector:** Custom U-shaped extruded gripper ($0.04\text{ m}$ extrusion depth). Configured as a **Convex Hull** for optimized Simscape collision physics.

---

##  Simulation Test Cases

### 1. Forward Kinematics Workspace Mapping (Sine Wave Input)
* **Configuration:** Harmonic sine wave inputs (amplitude $\pi/2$ rad, default frequencies) applied to all four revolute joints.
* **Sensing:** A Transform Sensor records real-time $X, Y, Z$ coordinates routed to a workspace array (`out.trajectory`) to map the reachable 3D workspace envelope.

### 2. Contact Dynamics & Environmental Collision
* **Configuration:** A rigid table fixture modeled via a Brick Solid ($0.50 \times 0.50 \times 0.02\text{ m}$) elevated to $Z = 0.30\text{ m}$.
* **Physics Handling:** Outer joints (Elbow and Wrist) were set to passive/unactuated states (`Motion: Automatically Computed`, `Torque: None`) to prevent infinite torque violations. A **Spatial Contact Force** block measures the impact normal force ($fn$) spike as the convex hull grips the table surface.

### 3. Singularity Mitigation & S-Curve Trajectory Generation
* **Problem:** Raw step inputs induce infinite accelerations at $t=0$, triggering solver step-size violations ($7.76 \times 10^{-15}$).
* **Solution:** Intermediary **Simulink-PS Converters** were configured with **Second-order filtering** ($\tau = 0.5\text{ s}$) and degree units (`deg`) to convert abrupt steps into smooth, continuous S-curve velocity and acceleration profiles.

---

##  Denavit-Hartenberg (D-H) Parameters

| Link ($i$) | $a_i$ (Length) | $\alpha_i$ (Twist) | $d_i$ (Offset) | $\theta_i$ (Joint Angle) |
| :--- | :--- | :--- | :--- | :--- |
| 1 (Base) | $0\text{ m}$ | $90^\circ$ | $0.10\text{ m}$ ($d_1 = L_{base}$) | $\theta_1$ |
| 2 (Shoulder) | $0.20\text{ m}$ ($a_2 = L_{upper}$) | $0^\circ$ | $0\text{ m}$ | $\theta_2$ |
| 3 (Elbow) | $0.10\text{ m}$ ($a_3 = L_{lower}$) | $0^\circ$ | $0\text{ m}$ | $\theta_3$ |
| 4 (Wrist) | $0\text{ m}$ | $0^\circ$ | $0\text{ m}$ | $\theta_4$ |

---

##  Requirements & Software
* MATLAB (R2023a or newer recommended)
* Simscape Multibody Toolbox

---
1. Clone the repository:
   ```bash
   git clone [https://github.com/your-username/4-dof-robotic-manipulator-simscape.git](https://github.com/your-username/4-dof-robotic-manipulator-simscape.git)
