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
\therefore \quad  H =  \sum_{\langle i,j \rangle} \left( J_x S_i^x \cdot S_j^x + J_y S_i^y \cdot S_j^y + J_z S_i^z \cdot S_j^z \right)
$$

We can now map $S_i^xS_j^x = \frac{1}{2} \sigma_i^x \sigma_j^x$, $S_i^yS_j^y = \frac{1}{2} \sigma_i^y \sigma_j^y$ , $S_i^zS_j^z = \frac{1}{2} \sigma_i^z \sigma_j^z$ ,

$$ 
H = \sum_{\langle i,j \rangle}\left(J_x X_iX_{j} + J_y Y_iY_{j} + J_z Z_iZ_{j}\right)
$$

We also introduce the term for the magnetic field of strength h, which will interact with each electron via the pauli X.

$$ 
H = \sum_{\langle i,j \rangle}\left(J_x X_iX_{j} + J_y Y_iY_{j} + J_z Z_iZ_{j} + hX_i \right)
$$

### Anisotrophy
We define the anisotrophy to be XXZ where $J=1$ and $\Delta = J_z/J$

So our hamiltonian further changes to,

$$ 
H = \sum_{\langle i,j \rangle}\left( X_iX_{j} + Y_iY_{j} + \Delta Z_iZ_{j} + hX_i \right)
$$

We will be running our simulations for two cases, Anisotrophic with $\Delta = -5$ and Isotrophic or XXX phase with $\Delta = 1$

## Implementation 

### Mapping spin chain to physical qubits

We define a coupling map using the `CouplingMap.from_line()` from qiskit.

### Generating the hamlitonian in form of pauli operators

`build_layered_hamiltonian(num_spins, anisotropy, h)` is the function which returns our hamiltonian in SparsePauliOps [[2]](https://docs.quantum.ibm.com/api/qiskit/qiskit.quantum_info.SparsePauliOp) customized on the given parameters.

### Obtaining the time evolution circuit

The time evolution operator of a hamiltonian is given by the formula,

$$
U(t) = e^{-iHt} ,
$$

and hence the time evolution operator for our hamiltonian becomes,

$$
U(t) = \exp \left( \sum_{\langle i,j \rangle}\left( X_iX_{j} + Y_iY_{j} + \Delta Z_iZ_{j} + hX_i \right) \right)
$$

We use the `PauliEvolutionGate()` function from qiskit [[3]](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.PauliEvolutionGate), to obtain action of $U(t)$ for some defined parameter $\delta t$,
Once we have our time evolution defined we get a trotterized approximation and synthesize it to a quantum circuit.

I used the `LieTrotter()` function from qiskit [[4]](https://docs.quantum.ibm.com/api/qiskit/qiskit.synthesis.LieTrotter) to obtain the approximation, and `synthesize` [[5]](https://docs.quantum.ibm.com/api/qiskit/synthesis) to get our final evolution circuit.

![render of the circuit](https://github.com/Celerium-Ce/Heisenberg_QSim/blob/main/imgs/hi.png)

### Observables for measuring 

Now to measure $M_{z}$ we must define our observables `SparsePauliOp.from_sparse_list()` [[2]](https://docs.quantum.ibm.com/api/qiskit/qiskit.quantum_info.SparsePauliOp) can be used to generate observables of form $I..ZII..I$.  

### Generating PUBs

We create pubs containing the paramaterized time evolution circuit, observables and value of parameter $\delta t$ 

## Results and Post Processing

$\delta t = \frac{5\pi}{2}$

$h$ varies as `[0 0.14279967 0.28559933 0.428399 0.57119866 0.71399833 0.856798 0.99959766 1.14239733 1.28519699 1.42799666 1.57079633]`

Average magnetization is computed as 


$$
M_{z} = \frac{1}{n} \sum_{i}^{n} M_z^i
$$

### Pass Manager Settings

`optimization_level=3` [[6]](https://docs.quantum.ibm.com/guides/configure-error-suppression)

### Results 
Graphs are made using `seaborn`,`matplotlib`

![IBM_Kyoto](https://github.com/Celerium-Ce/Heisenberg_QSim/blob/main/imgs/IBM_Sherbrooke.png)

![IBM_Osaka](https://github.com/Celerium-Ce/Heisenberg_QSim/blob/main/imgs/IBM_Osaka.png)

![IBM_Sherbrooke](https://github.com/Celerium-Ce/Heisenberg_QSim/blob/main/imgs/IBM_Sherbrooke.png)

![IBM_Sherbrooke](https://github.com/Celerium-Ce/Heisenberg_QSim/blob/main/imgs/IBM_Sherbrooke_2.png)








