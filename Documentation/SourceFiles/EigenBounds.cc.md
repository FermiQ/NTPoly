# `Source/CPlusPlus/EigenBounds.cc`

## Overview

This file provides C++ wrapper functions for estimating the eigenvalue bounds (minimum and maximum eigenvalues) of a given matrix. It implements two common techniques: the Gershgorin Circle Theorem for general bounds and a power iteration based method for the largest eigenvalue in magnitude. These functions serve as a C++ interface to underlying C routines, enabling their use with `NTPoly::Matrix_ps` objects.

## Key Components

All functions are static members of the `NTPoly::EigenBounds` class, which would be declared in `EigenBounds.h`.

- **`NTPoly::EigenBounds::GershgorinBounds(const Matrix_ps &matrix, double *min_value, double *max_value)`:**
  Calculates estimates for the minimum and maximum eigenvalues of the input `matrix` using the Gershgorin Circle Theorem.
  - `matrix`: The input matrix for which eigenvalue bounds are to be estimated.
  - `min_value`: A pointer to a double where the estimated lower bound of the eigenvalues will be stored.
  - `max_value`: A pointer to a double where the estimated upper bound of the eigenvalues will be stored.
  This function calls an external C wrapper `GershgorinBounds_wrp`.

- **`NTPoly::EigenBounds::PowerBounds(const Matrix_ps &matrix, double *max_value, const SolverParameters &solver_parameters)`:**
  Estimates the magnitude of the largest eigenvalue (spectral radius) of the input `matrix` using a power iteration method.
  - `matrix`: The input matrix.
  - `max_value`: A pointer to a double where the estimated magnitude of the maximum eigenvalue will be stored.
  - `solver_parameters`: Parameters to control the behavior of the power iteration solver.
  This function calls an external C wrapper `PowerBounds_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants. The primary data elements are the parameters passed to its functions (matrices, output pointers for bounds, and solver parameters).

## Usage Examples

No specific usage examples were found in the file. These functions are intended to be called by other C++ modules within the NTPoly project that require quick estimates of eigenvalue spectra.

```cc
// No specific usage examples were found in the file.
// A conceptual usage would be:

// NTPoly::Matrix_ps my_matrix;
// // ... (Initialize my_matrix) ...
// NTPoly::SolverParameters params;
// // ... (Configure params if necessary for PowerBounds) ...

// double gersh_min, gersh_max;
// NTPoly::EigenBounds::GershgorinBounds(my_matrix, &gersh_min, &gersh_max);
// // gersh_min and gersh_max now contain the Gershgorin bounds.

// double power_max_eig;
// NTPoly::EigenBounds::PowerBounds(my_matrix, &power_max_eig, params);
// // power_max_eig now contains an estimate of the largest eigenvalue magnitude.
```

## Dependencies and Interactions

- **Headers:**
  - `#include "EigenBounds.h"`: Includes the C++ class declaration for `NTPoly::EigenBounds`.
  - `#include "EigenBounds_c.h"`: Includes declarations for the C wrapper functions (`GershgorinBounds_wrp`, `PowerBounds_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects. The `GetIH()` method is used to retrieve underlying C handles for these matrices to pass to the C wrapper functions.
  - The `PowerBounds` method interacts with `NTPoly::SolverParameters` objects, also using `GetIH()` to pass them to its C wrapper.
- **External C Functions:** The core logic for both estimation methods is delegated to external C functions (suffixed with `_wrp`), indicating this file is primarily a C++ to C bridge.
```
