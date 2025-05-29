# `Source/CPlusPlus/ExponentialSolvers.cc`

## Overview

This file provides C++ wrapper functions for computing matrix exponentials (e<sup>A</sup>) and matrix logarithms (log(A)). It offers several algorithms for the matrix exponential, including a general-purpose method, a method optimized for dense matrices, and a Pade approximation method. For the matrix logarithm, both general and dense matrix versions are available. These functions serve as C++ interfaces to underlying C routines, facilitating their use with `NTPoly::Matrix_ps` objects.

## Key Components

All functions are static members of the `NTPoly::ExponentialSolvers` class, which is expected to be declared in `ExponentialSolvers.h`.

- **`NTPoly::ExponentialSolvers::ComputeExponential(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the matrix exponential of the `Input` matrix and stores the result in the `Output` matrix.
  - Calls the C wrapper: `ComputeExponential_wrp`.

- **`NTPoly::ExponentialSolvers::ComputeDenseExponential(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the matrix exponential of the `Input` matrix, presumably using an algorithm optimized for dense matrices. The result is stored in `Output`.
  - Calls the C wrapper: `ComputeDenseExponential_wrp`.

- **`NTPoly::ExponentialSolvers::ComputeExponentialPade(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the matrix exponential of the `Input` matrix using Pade approximants. The result is stored in `Output`.
  - Calls the C wrapper: `ComputeExponentialPade_wrp`.

- **`NTPoly::ExponentialSolvers::ComputeLogarithm(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the matrix logarithm of the `Input` matrix and stores the result in `Output`.
  - Calls the C wrapper: `ComputeLogarithm_wrp`.

- **`NTPoly::ExponentialSolvers::ComputeDenseLogarithm(const Matrix_ps &Input, Matrix_ps &Output, const SolverParameters &solver_parameters)`:**
  Computes the matrix logarithm of the `Input` matrix, presumably using an algorithm optimized for dense matrices. The result is stored in `Output`.
  - Calls the C wrapper: `ComputeDenseLogarithm_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants. The behavior of the functions is primarily determined by the input matrices and solver parameters.

## Usage Examples

No specific usage examples were found in the file. These solver functions are intended for use by other components within the NTPoly library that require matrix exponentiation or logarithm capabilities.

```cc
// No specific usage examples were found in the file.
// A conceptual usage might be:

// NTPoly::Matrix_ps A, expA, logA;
// // ... Initialize matrix A ...
// NTPoly::SolverParameters params;
// // ... Configure params ...

// // Compute matrix exponential
// NTPoly::ExponentialSolvers::ComputeExponential(A, expA, params);

// // Compute matrix logarithm
// NTPoly::ExponentialSolvers::ComputeLogarithm(A, logA, params);
```

## Dependencies and Interactions

- **Headers:**
  - `#include "ExponentialSolvers.h"`: Includes the C++ class declaration for `NTPoly::ExponentialSolvers`.
  - `#include "ExponentialSolvers_c.h"`: Includes declarations for the C wrapper functions (e.g., `ComputeExponential_wrp`, `ComputeLogarithm_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, representing input and output matrices. The `GetIH()` method is used to pass their C handles to the C wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, passed to control the behavior of the underlying solvers, also using `GetIH()`.
- **External C Functions:** The core computational logic for all solver routines is delegated to external C functions (suffixed with `_wrp`). This file provides the C++ interface to these C implementations.
```
