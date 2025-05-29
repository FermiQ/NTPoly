# `Source/CPlusPlus/TrigonometrySolvers.cc`

## Overview

This file provides C++ wrapper functions for computing matrix trigonometric functions, specifically the matrix sine (sin(A)) and matrix cosine (cos(A)). It offers implementations for general sparse matrices (`NTPoly::Matrix_ps`) as well as versions presumably optimized for dense matrices. These functions are part of the `NTPoly::TrigonometrySolvers` class and interface with underlying C routines for the core computations.

## Key Components

All functions are static members of the `NTPoly::TrigonometrySolvers` class, which is expected to be declared in `TrigonometrySolvers.h`.

- **`NTPoly::TrigonometrySolvers::Sine(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the matrix sine of the `Input` matrix and stores the result in the `Output` matrix.
  - `Input`: The input matrix.
  - `Output`: The matrix to store the computed sin(Input).
  - `solver_parameters`: Parameters to control the behavior of the algorithm.
  - Calls the C wrapper: `Sine_wrp`.

- **`NTPoly::TrigonometrySolvers::DenseSine(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the matrix sine of the `Input` matrix, presumably using an algorithm optimized for dense matrices. The result is stored in `Output`.
  - `Input`: The input matrix.
  - `Output`: The output matrix.
  - `solver_parameters`: Parameters for the dense sine algorithm.
  - Calls the C wrapper: `DenseSine_wrp`.

- **`NTPoly::TrigonometrySolvers::Cosine(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the matrix cosine of the `Input` matrix. The result is stored in `Output`.
  - `Input`: The input matrix.
  - `Output`: The matrix to store the computed cos(Input).
  - `solver_parameters`: Parameters to control the behavior of the algorithm.
  - Calls the C wrapper: `Cosine_wrp`.

- **`NTPoly::TrigonometrySolvers::DenseCosine(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the matrix cosine of the `Input` matrix, presumably using an algorithm optimized for dense matrices. The result is stored in `Output`.
  - `Input`: The input matrix.
  - `Output`: The output matrix.
  - `solver_parameters`: Parameters for the dense cosine algorithm.
  - Calls the C wrapper: `DenseCosine_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants. The behavior of the functions is primarily determined by the input matrices and solver parameters.

## Usage Examples

No specific usage examples were found in the file. Matrix sine and cosine functions are used in various areas of mathematics, physics, and engineering, often in solving systems of differential equations or in signal processing.

```cpp
// No specific usage examples were found in the file.
// A conceptual usage might be:

// NTPoly::Matrix_ps A, sinA, cosA;
// // ... Initialize matrix A ...
// NTPoly::SolverParameters params;
// // ... Configure params ...

// // Compute matrix sine
// NTPoly::TrigonometrySolvers::Sine(A, sinA, params);

// // Compute matrix cosine
// NTPoly::TrigonometrySolvers::Cosine(A, cosA, params);
```

## Dependencies and Interactions

- **Headers:**
  - `#include "TrigonometrySolvers.h"`: Includes the C++ class declaration for `NTPoly::TrigonometrySolvers`.
  - `#include "TrigonometrySolvers_c.h"`: Includes declarations for the C wrapper functions (e.g., `Sine_wrp`, `Cosine_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, representing input and output matrices. The `GetIH()` method is used to pass their C handles to the C wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, passed to control the behavior of the underlying solution algorithms, also using `GetIH()`.
- **External C Functions:** The core computational logic for matrix sine and cosine calculations is delegated to external C functions (suffixed with `_wrp`). This file provides the C++ interface.
```
