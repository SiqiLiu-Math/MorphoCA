# Models for MorphoCA

Morphogenesis is a biological process in multicellular organisms that governs the development of shape and structure. 
This complex system includes molecular regulations, mechanical dynamics, and feedback from the shape and structure. 
People used Cellular Automata to model the molecular regulation, and use vertex models for the mechanical dynamics.
Here, we combine these models to study how organisms use simple rules to complete morphological tasks.

## Model structure

Cells are the units in the model. Set the number of cells as $N_c$. And we use Greek indices ($\alpha, \beta, ...$) for cells.
For each cell, we consider the three properties: the molecular level, the mechanical states, and the geometry.
To construct the model, we will set up several rules:
- The cells update their molecular level according to the geometry: $Molecule(t+1) = R_1(Molecule(t), Geometry(t))$.
- The molecules regulate their mechanical states: $Mechanics(t) = R_2(Molecule(t))$.
- The geometry changes according to the mechanical states of cells: $Geometry(t+1) = R_3(Geometry(t), Mechanics(t))$.
- We also need an additional rule $R_4$ to govern cell division.

Here we suppose there are $N_m$ different kinds of molecules. So we can use a $N_m \times N_c$ matrix $(M_{i\alpha})$ to represent the current molecular states for the whole system. We set the range of molecule levels to be $M_{i\alpha}\in[0,1]$.

To be simple, we only consider two mechanical variables for each cell: the rest volume $V_{\alpha}$ and the membrane surface tension $T_{\alpha}$.

As for geometry, we suppose the cellular aggregate is a weighted Voronoi tessellation. So we need 3 geometric variables for each cell: the locations $(X_{\alpha},Y_{\alpha})$ and the weights $W_{\alpha}$. If we consider the 3D system, we need one more variable $Z_{\alpha}$.
