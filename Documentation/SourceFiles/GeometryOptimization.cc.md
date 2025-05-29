# `Source/CPlusPlus/GeometryOptimization.cc`

## Overview

This file provides C++ wrapper functions for methods commonly used in molecular geometry optimization procedures, specifically for extrapolating an electronic density matrix from a previous geometry step to the current geometry. This helps in providing a good initial guess for the Self-Consistent Field (SCF) calculation at the new geometry, potentially speeding up convergence. The file includes purification-based and Lowdin-based extrapolation techniques. These functions serve as C++ interfaces to underlying C routines.

## Key Components

All functions are static members of the `NTPoly::GeometryOptimization` class, which is expected to be declared in `GeometryOptimization.h`.

- **`NTPoly::GeometryOptimization::PurificationExtrapolate(const Matrix_ps &PreviousDensity, const Matrix_ps &Overlap, const double trace, Matrix_ps &NewDensity, const SolverParameters &solver_parameters)`:**
  Extrapolates the `PreviousDensity` matrix to generate a `NewDensity` matrix for the current geometry, using a purification-based method.
  - `PreviousDensity`: The density matrix from the previous geometry step.
  - `Overlap`: The overlap matrix corresponding to the current geometry.
  - `trace`: The target number of electrons (trace of the density matrix).
  - `NewDensity`: The output extrapolated density matrix for the current geometry.
  - `solver_parameters`: Parameters to control the extrapolation procedure.
  - Calls the C wrapper: `PurificationExtrapolate_wrp`.

- **`NTPoly::GeometryOptimization::LowdinExtrapolate(const Matrix_ps &PreviousDensity, const Matrix_ps &OldOverlap, const Matrix_ps &NewOverlap, Matrix_ps &NewDensity, const SolverParameters &solver_parameters)`:**
  Extrapolates the `PreviousDensity` matrix (associated with `OldOverlap`) to generate a `NewDensity` matrix for the current geometry (associated with `NewOverlap`), using a method based on Lowdin orthogonalization.
  - `PreviousDensity`: The density matrix from the previous geometry step.
  - `OldOverlap`: The overlap matrix from the previous geometry step.
  - `NewOverlap`: The overlap matrix for the current geometry.
  - `NewDensity`: The output extrapolated density matrix for the current geometry.
  - `solver_parameters`: Parameters to control the extrapolation procedure.
  - Calls the C wrapper: `LowdinExtrapolate_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants. The behavior of the functions is primarily determined by the input matrices (density and overlap matrices from different geometry steps) and solver parameters.

## Usage Examples

No specific usage examples were found in the file. These functions are intended to be integrated into a larger geometry optimization workflow, where they are called between geometry steps to provide initial guesses for SCF procedures.

```cc
// No specific usage examples were found in the file.
// A conceptual usage within a geometry optimization loop might be:

// NTPoly::Matrix_ps p_density_old, s_matrix_old, s_matrix_new, p_density_new_guess;
// // ... (Inside a geometry optimization loop) ...
// // After a geometry step, s_matrix_old and p_density_old are from the completed step.
// // s_matrix_new is for the newly updated geometry.
// NTPoly::SolverParameters geo_opt_params;
// // ... (Configure params) ...
// double num_electrons = 100.0; // Example

// // Using PurificationExtrapolate (assuming s_matrix_new is the current overlap)
// // NTPoly::GeometryOptimization::PurificationExtrapolate(p_density_old, s_matrix_new,
// //                                                     num_electrons, p_density_new_guess,
// //                                                     geo_opt_params);

// // Using LowdinExtrapolate
// NTPoly::GeometryOptimization::LowdinExtrapolate(p_density_old, s_matrix_old,
//                                                s_matrix_new, p_density_new_guess,
//                                                geo_opt_params);

// // p_density_new_guess can now be used as an initial guess for the SCF at the new geometry.
```

## Dependencies and Interactions

- **Headers:**
  - `#include "GeometryOptimization.h"`: Includes the C++ class declaration for `NTPoly::GeometryOptimization`.
  - `#include "PSMatrix.h"`: Likely included for the `NTPoly::Matrix_ps` type definition or related functionalities.
  - `#include "SolverParameters.h"`: Includes the definition of `NTPoly::SolverParameters`.
  - `#include "GeometryOptimization_c.h"`: Includes declarations for the C wrapper functions (`PurificationExtrapolate_wrp`, `LowdinExtrapolate_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, representing density matrices and overlap matrices from different stages of a geometry optimization. The `GetIH()` method is used to pass their C handles to the C wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, passed to control the behavior of the underlying extrapolation algorithms, also using `GetIH()`.
- **External C Functions:** The core computational logic for these extrapolation methods is delegated to external C functions (suffixed with `_wrp`). This file provides the C++ interface to these C implementations.
```
