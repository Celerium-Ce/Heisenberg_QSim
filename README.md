# Heisenberg_QSim
A quantum simulation of many body electron-spin chains to observe its magnetization dynamics. This is also performed to observe how a quantum computer can be used to run simulations which are quantum in nature more efficiently than classical computers.

I use qiskit to perform these simulations on IBM's Superconducting Quantum Computers.

## The Problem Statement
Simulate a 50 electron heisenberg spin chain in presence of an external magnetic field, and oberserve the relation between M<sub>Z</sub> and the magitude of the external magnetic field.
### The Heisenberg Hamiltonian
We have the generalized heisenberg hamiltonian [[1]](https://iopscience.iop.org/book/mono/978-0-7503-3879-0/chapter/bk978-0-7503-3879-0ch1.pdf):

$$
H =  \frac{1}{2} \sum_{ij} J_{ij} \vec{S}_i \cdot \vec{S}_j 
$$

Here the term ij runs over the entire lattice, but since the problem we are modelling is a chain and for the sake of simplicity we only take the interactions of the nearest neighbours, hence we can update the hamiltonian to be 

$$
H =  \sum_{\langle i,j \rangle} J_{ij} \vec{S}_i \cdot \vec{S}_j 
$$

where $\langle i,j \rangle$ represents sum over nearest neighbours

Now,

$$ 
J_{ij} \vec{S}_i \cdot \vec{S}_j = J_x S_i^x \cdot S_j^x + J_y S_i^y \cdot S_j^y + J_z S_i^z \cdot S_j^z
$$


$$
\therefore  H =  \sum_{\langle i,j \rangle} \left( J_x S_i^x \cdot S_j^x + J_y S_i^y \cdot S_j^y + J_z S_i^z \cdot S_j^z \right)
$$

We can now map $S_i^xS_j^x = \frac{1}{2} \sigma_i^x \sigma_j^x$, $S_i^yS_j^y = \frac{1}{2} \sigma_i^y \sigma_j^y$ , $S_i^zS_j^z = \frac{1}{2} \sigma_i^z \sigma_j^z$ ,

$$ 
H = \sum_{\langle i,j \rangle}\left(J_x X_iX_{j} + J_y Y_iY_{j} + J_z Z_iZ_{j}\right)
$$

We also introduce the term for the magnetic field of strength h, which will interact with each electron via the pauli X.

$$ 
H = \sum_{\langle i,j \rangle}\left(J_x X_iX_{j} + J_y Y_iY_{j} + J_z Z_iZ_{j} + hX_i \right)
$$

#### Anisotrophy
We define the anisotrophy to be XXZ where $J=1$ and $\Delta = J_z/J$

So our hamiltonian further changes to,

$$ 
H = \sum_{\langle i,j \rangle}\left( X_iX_{j} + Y_iY_{j} + \Delta Z_iZ_{j} + hX_i \right)
$$

We will be running our simulations for two cases Anisotrophic $\Delta = -5$ and XXX phase $\Delta = 1$


