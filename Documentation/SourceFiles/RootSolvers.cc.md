# `Source/CPlusPlus/RootSolvers.cc`

## Overview

This file provides C++ wrapper functions for computing arbitrary integer roots and inverse integer roots of matrices. Specifically, it allows for the calculation of A<sup>1/n</sup> and A<sup>-1/n</sup> where 'n' is an integer root. These operations are part of the `NTPoly::RootSolvers` class and interface with underlying C routines for the actual computation.

## Key Components

All functions are static members of the `NTPoly::RootSolvers` class, which is expected to be declared in `RootSolvers.h`.

- **`NTPoly::RootSolvers::ComputeRoot(const Matrix_ps &InputMat, Matrix_ps &OutputMat, int root, const SolverParameters &solver_parameters)`:**
  Computes the n-th root of the `InputMat`, where 'n' is specified by the `root` parameter (e.g., if `root` is 2, it computes the square root). The result is stored in `OutputMat`.
  - `InputMat`: The input matrix.
  - `OutputMat`: The matrix to store the computed root.
  - `root`: An integer specifying the desired root (e.g., 2 for square root, 3 for cube root).
  - `solver_parameters`: Parameters to control the behavior of the root-finding algorithm.
  - Calls the C wrapper: `ComputeRoot_wrp`.

- **`NTPoly::RootSolvers::ComputeInverseRoot(const Matrix_ps &InputMat, Matrix_ps &OutputMat, int root, const SolverParameters &solver_parameters)`:**
  Computes the inverse n-th root of the `InputMat` (i.e., (A<sup>1/n</sup>)<sup>-1</sup> or A<sup>-1/n</sup>), where 'n' is specified by the `root` parameter. The result is stored in `OutputMat`.
  - `InputMat`: The input matrix.
  - `OutputMat`: The matrix to store the computed inverse root.
  - `root`: An integer specifying the desired root.
  - `solver_parameters`: Parameters to control the behavior of the algorithm.
  - Calls the C wrapper: `ComputeInverseRoot_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants. The most significant parameter specific to these functions is `root`, which dictates the order of the root to be computed.

## Usage Examples

No specific usage examples were found in the file. These functions would be used in contexts requiring matrix root calculations, such as in some transformations or orthogonalization procedures.

```cpp
// No specific usage examples were found in the file.
// A conceptual usage might be:

// NTPoly::Matrix_ps A, A_sqrt, A_inv_cbrt;
// // ... Initialize matrix A ...
// NTPoly::SolverParameters params;
// // ... Configure params ...

// // Compute square root of A (A^(1/2))
// NTPoly::RootSolvers::ComputeRoot(A, A_sqrt, 2, params);

// // Compute inverse cube root of A (A^(-1/3))
// NTPoly::RootSolvers::ComputeInverseRoot(A, A_inv_cbrt, 3, params);
```

## Dependencies and Interactions

- **Headers:**
  - `#include "RootSolvers.h"`: Includes the C++ class declaration for `NTPoly::RootSolvers`.
  - `#include "RootSolvers_c.h"`: Includes declarations for the C wrapper functions (`ComputeRoot_wrp`, `ComputeInverseRoot_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, representing input and output matrices. The `GetIH()` method is used to pass their C handles to the C wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, passed to control the behavior of the underlying solution algorithms, also using `GetIH()`.
- **External C Functions:** The core computational logic for root and inverse root calculations is delegated to external C functions (suffixed with `_wrp`). This file provides the C++ interface.
```
