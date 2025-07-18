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

To be simple, we only consider two mechanical variables for each cell: the rest volume size $S_{\alpha}$ and the membrane surface tension $T_{\alpha}$.

As for geometry, we suppose the cellular aggregate is a weighted Voronoi tessellation. So we need 3 geometric variables for each cell: the Voronoi nodes $(X_{\alpha},Y_{\alpha})$ and the weights $U_{\alpha}$. If we consider the 3D system, we need one more variable $Z_{\alpha}$ for the nodes.

## Model and parameters

### Molecule level update - Graph Neural Cellular Automata

In the classic Cellular Automata (CA), each cell updates their states according to the states of its nearest neighbors (NNs). However, in classic CA, the NNs are fixed and regular, where cells are placed as 2D grids. The Graph Cellular Automata (GCA) considers more general topology of cells, where the cell's NNs is given by graph edges.
In our model, the NNs pass molecular signals to the target cells by summing molecular levels weighted by contact areas:

$$ \tilde{M_{i\alpha}} = \sum_{\beta} M_{i\beta}A_{\alpha\beta} $$

Here, $\tilde{M}$ represents the received signals, $A_{\alpha\beta}$ is the entry of the adjacency matrix representing the contact area between cell $\alpha$ and $\beta$.

To update the molecule states $M \rightarrow M'$, we use a one-layer neural network:

$$ M'=\sigma(W M + V \tilde{M} + B ) $$

Here, $W$ and $V$ are weight matrices ($N_m\times N_m$), and $B$ is the bias. And $\sigma$ represents the logistic function $\sigma(x)=1/(1+e^{-s})$.

### Molecules regulate mechanics

To be simple, we assume there are two molecules ($i=1,2$) directly regulating two mechanical variables ($S_\alpha,T_\alpha$), respectively. We also assume the mechanical variables linearly depend on molecule levels.

$$ S_\alpha = S_0+k_S M_{1\alpha} $$

$$ T_\alpha = T_0+k_T M_{2\alpha} $$

### Mechanics drive the change of geometry



