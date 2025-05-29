# `Source/CPlusPlus/SquareRootSolvers.cc`

## Overview

This file provides C++ wrapper functions for computing the matrix square root (A<sup>1/2</sup>) and the matrix inverse square root (A<sup>-1/2</sup>). It offers implementations for general sparse matrices (`NTPoly::Matrix_ps`) as well as versions presumably optimized for dense matrices. These functions are part of the `NTPoly::SquareRootSolvers` class and interface with underlying C routines for the core computations.

## Key Components

All functions are static members of the `NTPoly::SquareRootSolvers` class, which is expected to be declared in `SquareRootSolvers.h`.

- **`NTPoly::SquareRootSolvers::SquareRoot(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the principal square root of the `Input` matrix and stores the result in the `Output` matrix.
  - `Input`: The input matrix (must be positive semi-definite or meet solver-specific criteria).
  - `Output`: The matrix to store the computed square root.
  - `solver_parameters`: Parameters to control the behavior of the square root algorithm.
  - Calls the C wrapper: `SquareRoot_wrp`.

- **`NTPoly::SquareRootSolvers::DenseSquareRoot(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the square root of the `Input` matrix, presumably using an algorithm optimized for dense matrices. The result is stored in `Output`.
  - `Input`: The input matrix.
  - `Output`: The output matrix.
  - `solver_parameters`: Parameters for the dense square root algorithm.
  - Calls the C wrapper: `DenseSquareRoot_wrp`.

- **`NTPoly::SquareRootSolvers::InverseSquareRoot(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the inverse principal square root of the `Input` matrix (A<sup>-1/2</sup>). The result is stored in `Output`.
  - `Input`: The input matrix (must be positive definite or meet solver-specific criteria).
  - `Output`: The matrix to store the computed inverse square root.
  - `solver_parameters`: Parameters to control the behavior of the algorithm.
  - Calls the C wrapper: `InverseSquareRoot_wrp`.

- **`NTPoly::SquareRootSolvers::DenseInverseSquareRoot(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the inverse square root of the `Input` matrix, presumably using an algorithm optimized for dense matrices. The result is stored in `Output`.
  - `Input`: The input matrix.
  - `Output`: The output matrix.
  - `solver_parameters`: Parameters for the dense inverse square root algorithm.
  - Calls the C wrapper: `DenseInverseSquareRoot_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants. The behavior of the functions is primarily determined by the input matrices and solver parameters.

## Usage Examples

No specific usage examples were found in the file. These functions are fundamental in various numerical linear algebra contexts, such as matrix orthogonalization (e.g., Lowdin orthogonalization which uses A<sup>-1/2</sup>), solving generalized eigenvalue problems, or in statistical analysis.

```cpp
// No specific usage examples were found in the file.
// A conceptual usage might be:

// NTPoly::Matrix_ps S, S_inv_sqrt, S_sqrt;
// // ... Initialize matrix S (e.g., an overlap matrix) ...
// NTPoly::SolverParameters params;
// // ... Configure params ...

// // Compute inverse square root of S (S^(-1/2))
// NTPoly::SquareRootSolvers::InverseSquareRoot(S, S_inv_sqrt, params);

// // Compute square root of S (S^(1/2))
// NTPoly::SquareRootSolvers::SquareRoot(S, S_sqrt, params);
```

## Dependencies and Interactions

- **Headers:**
  - `#include "SquareRootSolvers.h"`: Includes the C++ class declaration for `NTPoly::SquareRootSolvers`.
  - `#include "SquareRootSolvers_c.h"`: Includes declarations for the C wrapper functions (e.g., `SquareRoot_wrp`, `InverseSquareRoot_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, representing input and output matrices. The `GetIH()` method is used to pass their C handles to the C wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, passed to control the behavior of the underlying solution algorithms, also using `GetIH()`.
- **External C Functions:** The core computational logic for square root and inverse square root calculations is delegated to external C functions (suffixed with `_wrp`). This file provides the C++ interface.
```
