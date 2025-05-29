# `Source/CPlusPlus/InverseSolvers.cc`

## Overview

This file provides C++ wrapper functions for various matrix inversion routines. It includes methods for standard matrix inversion (potentially for general sparse or specialized matrix types), inversion optimized for dense matrices, and computation of the Moore-Penrose pseudo-inverse. These functions act as C++ interfaces to underlying C implementations, allowing them to be used with `NTPoly::Matrix_ps` objects and `NTPoly::SolverParameters`.

## Key Components

All functions are static members of the `NTPoly::InverseSolvers` class, which is expected to be declared in `InverseSolvers.h`.

- **`NTPoly::InverseSolvers::Invert(const Matrix_ps &Overlap, Matrix_ps &InverseMat, const SolverParameters &solver_parameters)`:**
  Computes the inverse of the input `Overlap` matrix (often representing an overlap matrix S, but can be any invertible matrix) and stores the result in `InverseMat`.
  - `Overlap`: The input matrix to be inverted.
  - `InverseMat`: The output matrix to store the computed inverse.
  - `solver_parameters`: Parameters to control the inversion algorithm's behavior.
  - Calls the C wrapper: `Invert_wrp`.

- **`NTPoly::InverseSolvers::DenseInvert(const Matrix_ps &Overlap, Matrix_ps &InverseMat, const SolverParameters &solver_parameters)`:**
  Computes the inverse of the input `Overlap` matrix, presumably using an algorithm optimized for dense matrices. The result is stored in `InverseMat`.
  - `Overlap`: The input dense matrix to be inverted.
  - `InverseMat`: The output matrix to store the computed inverse.
  - `solver_parameters`: Parameters to control the inversion algorithm's behavior.
  - Calls the C wrapper: `DenseInvert_wrp`.

- **`NTPoly::InverseSolvers::PseudoInverse(const Matrix_ps &Overlap, Matrix_ps &InverseMat, const SolverParameters &solver_parameters)`:**
  Computes the Moore-Penrose pseudo-inverse of the input `Overlap` matrix. This is useful for non-square matrices or singular square matrices. The result is stored in `InverseMat`.
  - `Overlap`: The input matrix for which the pseudo-inverse is to be computed.
  - `InverseMat`: The output matrix to store the computed pseudo-inverse.
  - `solver_parameters`: Parameters to control the pseudo-inversion algorithm's behavior.
  - Calls the C wrapper: `PseudoInverse_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants. The behavior of the functions is primarily determined by the input matrices and solver parameters.

## Usage Examples

No specific usage examples were found in the file. These solver functions are intended for use by other routines within the NTPoly library that require matrix inversion.

```cc
// No specific usage examples were found in the file.
// A conceptual usage might be:

// NTPoly::Matrix_ps S_matrix, S_inv_matrix;
// // ... Initialize S_matrix ...
// NTPoly::SolverParameters params;
// // ... Configure params ...

// // Compute standard inverse
// NTPoly::InverseSolvers::Invert(S_matrix, S_inv_matrix, params);

// // Compute dense inverse if S_matrix is dense
// // NTPoly::InverseSolvers::DenseInvert(S_matrix, S_inv_matrix, params);

// // Compute pseudo-inverse
// // NTPoly::InverseSolvers::PseudoInverse(S_matrix, S_inv_matrix, params);
```

## Dependencies and Interactions

- **Headers:**
  - `#include "InverseSolvers.h"`: Includes the C++ class declaration for `NTPoly::InverseSolvers`.
  - `#include "InverseSolvers_c.h"`: Includes declarations for the C wrapper functions (e.g., `Invert_wrp`, `DenseInvert_wrp`, `PseudoInverse_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, representing input matrices and their inverses. The `GetIH()` method is used to pass their C handles to the C wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, passed to control the behavior of the underlying inversion algorithms, also using `GetIH()`.
- **External C Functions:** The core computational logic for all inversion routines is delegated to external C functions (suffixed with `_wrp`). This file provides the C++ interface to these C implementations.
```
