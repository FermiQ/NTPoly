# `Source/CPlusPlus/DensityMatrixSolvers.cc`

## Overview

This file implements C++ wrapper functions for a variety of algorithms aimed at computing the electronic density matrix. These methods are fundamental in quantum chemistry and materials science simulations. The functions provided here serve as an interface between the C++ environment of NTPoly (using `Matrix_ps` objects) and underlying C routines that perform the actual computations. Common methods like PM, TRS2, TRS4, HPCP, as well as dense matrix methods and spectrum folding techniques, are included. Additionally, it provides utilities for computing the energy density matrix and performing McWeeny purification steps.

## Key Components

All functions are static members of the `NTPoly::DensityMatrixSolvers` class, defined in the corresponding header file.

- **`NTPoly::DensityMatrixSolvers::PM(const Matrix_ps &Hamiltonian, const Matrix_ps &Overlap, double trace, Matrix_ps &Density, double &energy_value_out, double &chemical_potential_out, const SolverParameters &solver_parameters)`:**
  Computes the density matrix using a PM (Purification of the Density Matrix) type method.
  - Inputs: `Hamiltonian`, `Overlap` matrix, target electron count `trace`, `solver_parameters`.
  - Outputs: `Density` matrix, `energy_value_out`, `chemical_potential_out`.
  - Calls C wrapper: `PM_wrp`.

- **`NTPoly::DensityMatrixSolvers::TRS2(...)`:**
  Computes the density matrix using the TRS2 (Trace-Resetting Squared) purification method. Inputs and outputs are similar to `PM`.
  - Calls C wrapper: `TRS2_wrp`.

- **`NTPoly::DensityMatrixSolvers::TRS4(...)`:**
  Computes the density matrix using the TRS4 (Trace-Resetting Quartic) purification method. Inputs and outputs are similar to `PM`.
  - Calls C wrapper: `TRS4_wrp`.

- **`NTPoly::DensityMatrixSolvers::HPCP(...)`:**
  Computes the density matrix using an HPCP (Hybrid Pole-Zero Chebyshev Polynomial) or similar polynomial expansion method. Inputs and outputs are similar to `PM`.
  - Calls C wrapper: `HPCP_wrp`.

- **`NTPoly::DensityMatrixSolvers::DenseDensity(...)`:**
  Computes the density matrix using methods suitable for dense matrices. Inputs and outputs are similar to `PM`.
  - Calls C wrapper: `DenseDensity_wrp`.

- **`NTPoly::DensityMatrixSolvers::ScaleAndFold(const Matrix_ps &Hamiltonian, const Matrix_ps &Overlap, double trace, Matrix_ps &Density, double homo, double lumo, double &energy_value_out, const SolverParameters &solver_parameters)`:**
  Computes the density matrix by scaling and folding the energy spectrum, typically using provided HOMO (Highest Occupied Molecular Orbital) and LUMO (Lowest Unoccupied Molecular Orbital) energy estimates.
  - Calls C wrapper: `ScaleAndFold_wrp`.

- **`NTPoly::DensityMatrixSolvers::EnergyDensityMatrix(const Matrix_ps &Hamiltonian, const Matrix_ps &Density, Matrix_ps &EnergyDensity, double threshold)`:**
  Computes the energy-weighted density matrix (often P*H*P or similar) from the `Hamiltonian` and `Density` matrices. A `threshold` may be used for filtering or numerical stability.
  - Calls C wrapper: `EnergyDensityMatrix_wrp`.

- **`NTPoly::DensityMatrixSolvers::McWeenyStep(const Matrix_ps &D, Matrix_ps &DOut, double threshold)`:**
  Performs a McWeeny purification step (e.g., D_out = 3D^2 - 2D^3) on the input matrix `D` to produce `DOut`, aiming to restore idempotency.
  - Calls C wrapper: `McWeenyStep_wrp`.

- **`NTPoly::DensityMatrixSolvers::McWeenyStep(const Matrix_ps &D, const Matrix_ps &S, Matrix_ps &DOut, double threshold)`:**
  Performs a McWeeny purification step for non-orthogonal basis sets, involving the overlap matrix `S` (e.g., D_out = D S D).
  - Calls C wrapper: `McWeenyStepS_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants that affect its behavior. The primary data elements are the parameters passed to its functions, such as matrices, scalar values (trace, energy, chemical potential, thresholds, HOMO/LUMO), and solver parameter objects.

## Usage Examples

No specific usage examples were found in the file. These functions are intended to be called by higher-level C++ modules within the NTPoly project that perform electronic structure calculations.

```cc
// No specific usage examples were found in the file.
// A conceptual usage for one of the solvers might be:

// NTPoly::Matrix_ps hamiltonian, overlap, density_matrix;
// // ... (Initialize hamiltonian, overlap, density_matrix, often with ProcessGrid and MemoryPool) ...
// double target_trace = 100.0; // Example: number of electrons
// double energy, chemical_potential;
// NTPoly::SolverParameters params;
// // ... (Configure params if necessary) ...

// NTPoly::DensityMatrixSolvers::TRS2(hamiltonian, overlap, target_trace,
//                                   density_matrix, energy, chemical_potential,
//                                   params);

// // density_matrix, energy, and chemical_potential are now populated.
```

## Dependencies and Interactions

- **Headers:**
  - `#include "DensityMatrixSolvers.h"`: Includes the C++ class declaration for `NTPoly::DensityMatrixSolvers`.
  - `#include "PSMatrix.h"`: Included for `NTPoly::Matrix_ps` type (or a base it inherits from / uses).
  - `#include "SolverParameters.h"`: Includes the definition of `NTPoly::SolverParameters`.
  - `#include "DensityMatrixSolvers_c.h"`: Includes declarations for the external C wrapper functions (e.g., `PM_wrp`, `TRS2_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts heavily with `NTPoly::Matrix_ps` objects, representing Hamiltonians, overlap matrices, and density matrices. The `GetIH()` method is used to pass their C handles to the C wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, which are passed to most solver routines to control their behavior, also using `GetIH()`.
- **External C Functions:** The core computational logic for each solver and utility function is delegated to external C functions (typically suffixed with `_wrp`). This file provides the C++ interface to these C implementations.
```
