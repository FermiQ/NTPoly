# `Source/CPlusPlus/FermiOperator.cc`

## Overview

This file provides C++ wrapper functions for computing the Fermi-Dirac operator, which is crucial for determining the electronic density matrix at a finite temperature (non-zero `inv_temp`). It includes implementations presumably optimized for dense matrices (`ComputeDenseFOE`) and methods referred to as "WOM" (Window Operations Method or similar) tailored for both Grand Canonical (`WOM_GC`, fixed chemical potential) and Canonical (`WOM_C`, fixed number of electrons) ensembles. These functions serve as C++ interfaces to underlying C routines.

## Key Components

All functions are static members of the `NTPoly::FermiOperator` class, which is expected to be declared in `FermiOperator.h`.

- **`NTPoly::FermiOperator::ComputeDenseFOE(const Matrix_ps &Hamiltonian, const Matrix_ps &Overlap, double trace, Matrix_ps &Density, double inv_temp, double &energy_value_out, double &chemical_potential_out, const SolverParameters &solver_parameters)`:**
  Computes the Fermi Operator Expansion (FOE) to obtain the density matrix, presumably using algorithms suitable for dense matrices.
  - Inputs: `Hamiltonian` matrix, `Overlap` matrix (can be identity for orthogonal basis), target electron count `trace`, inverse temperature `inv_temp`, and `solver_parameters`.
  - Outputs: Computed `Density` matrix, `energy_value_out`, and `chemical_potential_out`.
  - Calls the C wrapper: `ComputeDenseFOE_wrp`.

- **`NTPoly::FermiOperator::WOM_GC(const Matrix_ps &Hamiltonian, const Matrix_ps &Overlap, Matrix_ps &Density, double chemical_potential, double inv_temp, double &energy_value_out, const SolverParameters &solver_parameters)`:**
  Computes the Fermi operator using a "WOM" method within the Grand Canonical ensemble (fixed chemical potential).
  - Inputs: `Hamiltonian`, `Overlap` matrix, `chemical_potential`, `inv_temp`, and `solver_parameters`.
  - Outputs: Computed `Density` matrix and `energy_value_out`.
  - Calls the C wrapper: `WOM_GC_wrp`.

- **`NTPoly::FermiOperator::WOM_C(const Matrix_ps &Hamiltonian, const Matrix_ps &Overlap, Matrix_ps &Density, double trace, double inv_temp, double &energy_value_out, const SolverParameters &solver_parameters)`:**
  Computes the Fermi operator using a "WOM" method within the Canonical ensemble (fixed number of electrons, specified by `trace`).
  - Inputs: `Hamiltonian`, `Overlap` matrix, target electron count `trace`, `inv_temp`, and `solver_parameters`.
  - Outputs: Computed `Density` matrix and `energy_value_out`.
  - Calls the C wrapper: `WOM_C_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants. Key input parameters that significantly affect the calculations are:
- `inv_temp`: The inverse temperature (1/(kT)). A value of 0 typically implies zero temperature behavior.
- `trace`: The desired number of electrons (for canonical ensemble methods).
- `chemical_potential`: The chemical potential (for grand canonical ensemble methods).

## Usage Examples

No specific usage examples were found in the file. These functions are intended for use within larger electronic structure calculation workflows in the NTPoly library.

```cc
// No specific usage examples were found in the file.
// A conceptual usage might be:

// NTPoly::Matrix_ps H, S, D_out;
// // ... Initialize H, S, D_out ...
// NTPoly::SolverParameters params;
// // ... Configure params ...
// double target_trace = 100.0; // For WOM_C or ComputeDenseFOE
// double inverse_temperature = 10.0; // Beta = 1/(kT)
// double energy, chemical_potential_val; // chemical_potential_val for ComputeDenseFOE output

// // Example for ComputeDenseFOE
// NTPoly::FermiOperator::ComputeDenseFOE(H, S, target_trace, D_out, inverse_temperature,
//                                       energy, chemical_potential_val, params);

// // Example for WOM_C (Canonical)
// // NTPoly::FermiOperator::WOM_C(H, S, D_out, target_trace, inverse_temperature,
// //                             energy, params);

// // Example for WOM_GC (Grand Canonical)
// // double mu = 0.1; // Chemical potential
// // NTPoly::FermiOperator::WOM_GC(H, S, D_out, mu, inverse_temperature,
// //                              energy, params);
```

## Dependencies and Interactions

- **Headers:**
  - `#include "FermiOperator.h"`: Includes the C++ class declaration for `NTPoly::FermiOperator`.
  - `#include "PSMatrix.h"`: Likely included for the `NTPoly::Matrix_ps` type definition or related functionalities.
  - `#include "SolverParameters.h"`: Includes the definition of `NTPoly::SolverParameters`.
  - `#include "FermiOperator_c.h"`: Includes declarations for the C wrapper functions (e.g., `ComputeDenseFOE_wrp`, `WOM_GC_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, representing Hamiltonians, overlap matrices, and density matrices. The `GetIH()` method is used to pass their C handles to the C wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, passed to control the behavior of the underlying solvers, also using `GetIH()`.
- **External C Functions:** The core computational logic for these Fermi operator calculations is delegated to external C functions (suffixed with `_wrp`). This file provides the C++ interface to these C implementations.
```
