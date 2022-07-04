# Explore Time Delay Interferometry 

This repository provides the simulation of six scientific measurements of a static constellation of *TAIJI*,  the TDI-1 computation, the TDI-Infinity computation, the APCI computation and the corresponding sensitivity curves.

- **Simulation** ([Baghi](https://journals.aps.org/prd/abstract/10.1103/PhysRevD.104.122001 ))

  Three *TAIJI* satellites rotates around the Sun, and they form an equilateral triangle whose centre follows a heliocentric circular orbit。

  *Simulation of the six measurements is not an end-to-end version. NSSC, LISA-Node and LISA-Code would offer it.*

- **TDI-1.0**

  X, A, E, T; requiring the light travel time between two satellites

- **TDI-Infinity** ([Vallisneri](https://arxiv.org/abs/2008.12343 ))

  The null space of the design matrix; requiring the light travel time between two satellites

- **APCI** ([Baghi](https://journals.aps.org/prd/abstract/10.1103/PhysRevD.104.122001 ))

  The principal component of the Toeplitz matrix of the science measurements. Not requiring the light travel time between two satellites. Of course, SVD and PCA are the same from a mathematical point of view. Additionally, PCA for TDI gives us a hint that we can handle TDI from a probability perspective. When it comes to the missing data case, three possible solutions are given: **imputation, alternative and considering**. 

  PPCA with EM algorithm may be promising; weight PCA is also worthy to try.

- **Sensitivity** ([Wang](https://journals.aps.org/prd/abstract/10.1103/PhysRevD.103.063021))

  Ok~ The equation used to compute the sensitivity curve is from WANGPANPAN(2020). **HOWEVER**, equations in the paper are inconsistent . 😅(HUST ~LoL~) Would try to compute the sensitivity curves from simulation.

**未经允许，请勿转载~**
