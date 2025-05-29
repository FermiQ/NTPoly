# `Source/CPlusPlus/LinearSolvers.cc`

## Overview

This file provides C++ wrapper functions for common linear algebra operations, specifically for solving systems of linear equations and performing Cholesky decomposition. It includes a Conjugate Gradient (CG) solver for systems of the form AX=B, and a routine for Cholesky decomposition (A = LL<sup>T</sup>) of symmetric positive-definite matrices. These functions serve as C++ interfaces to underlying C implementations, designed for use with `NTPoly::Matrix_ps` objects.

## Key Components

All functions are static members of the `NTPoly::LinearSolvers` class, which is expected to be declared in `LinearSolvers.h`.

- **`NTPoly::LinearSolvers::CGSolver(const Matrix_ps &AMat, Matrix_ps &XMat, const Matrix_ps &BMat, const SolverParameters &solver_parameters)`:**
  Solves the system of linear equations AX = B for X, where A is represented by `AMat`, B by `BMat`, and the solution X is stored in `XMat`. The Conjugate Gradient method is used for the solution.
  - `AMat`: The coefficient matrix A of the linear system.
  - `XMat`: The output matrix where the solution X will be stored.
  - `BMat`: The right-hand side matrix B of the linear system.
  - `solver_parameters`: Parameters to control the behavior and convergence of the CG solver.
  - Calls the C wrapper: `CGSolver_wrp`.

- **`NTPoly::LinearSolvers::CholeskyDecomposition(const Matrix_ps &AMat, Matrix_ps &LMat, const SolverParameters &solver_parameters)`:**
  Performs the Cholesky decomposition of a symmetric positive-definite matrix `AMat`. The decomposition is A = LL<sup>T</sup>, where L is a lower triangular matrix stored in `LMat`.
  - `AMat`: The input symmetric positive-definite matrix A to be decomposed.
  - `LMat`: The output matrix to store the lower triangular factor L.
  - `solver_parameters`: Parameters to control the Cholesky decomposition algorithm.
  - Calls the C wrapper: `CholeskyDecomposition_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants. The behavior of the functions is primarily determined by the input matrices and solver parameters.

## Usage Examples

No specific usage examples were found in the file. These solver functions are typically used as components in larger numerical algorithms.

```cc
// No specific usage examples were found in the file.
// A conceptual usage might be:

// NTPoly::Matrix_ps A_matrix, x_solution, b_rhs, L_factor;
// // ... Initialize A_matrix, b_rhs (for CG) or A_matrix (for Cholesky) ...
// NTPoly::SolverParameters params;
// // ... Configure params ...

// // Solve AX = B using Conjugate Gradient
// // NTPoly::LinearSolvers::CGSolver(A_matrix, x_solution, b_rhs, params);
// // x_solution now contains the solution X.

// // Perform Cholesky Decomposition (A = LL^T)
// // NTPoly::LinearSolvers::CholeskyDecomposition(A_matrix, L_factor, params);
// // L_factor now contains the lower triangular matrix L.
```

## Dependencies and Interactions

- **Headers:**
  - `#include "LinearSolvers.h"`: Includes the C++ class declaration for `NTPoly::LinearSolvers`.
  - `#include "LinearSolvers_c.h"`: Includes declarations for the C wrapper functions (`CGSolver_wrp`, `CholeskyDecomposition_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, representing coefficient matrices, right-hand side vectors/matrices, solution vectors/matrices, and Cholesky factors. The `GetIH()` method is used to pass their C handles to the C wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, passed to control the behavior of the underlying solution algorithms, also using `GetIH()`.
- **External C Functions:** The core computational logic for both the CG solver and Cholesky decomposition is delegated to external C functions (suffixed with `_wrp`). This file provides the C++ interface to these C implementations.
```
