# `Source/CPlusPlus/EigenSolvers.cc`

## Overview

This file provides C++ wrapper functions for various matrix eigensolver routines. It includes methods for full eigenvalue decomposition (eigenvalues and eigenvectors), computing eigenvalues only, performing Singular Value Decomposition (SVD), and estimating the HOMO-LUMO gap. These functions act as C++ interfaces to underlying C implementations, allowing seamless integration with `NTPoly::Matrix_ps` objects and solver parameters within the NTPoly framework.

## Key Components

All functions are static members of the `NTPoly::EigenSolvers` class, which is expected to be declared in `EigenSolvers.h`.

- **`NTPoly::EigenSolvers::EigenDecomposition(const Matrix_ps &matrix, Matrix_ps &eigenvalues, int nvals, Matrix_ps &eigenvectors, const SolverParameters &solver_parameters)`:**
  Performs eigenvalue decomposition on the input `matrix` to compute a specified number (`nvals`) of eigenvalues and their corresponding eigenvectors.
  - `matrix`: The input matrix for decomposition.
  - `eigenvalues`: Output matrix (typically a vector) to store the computed eigenvalues.
  - `nvals`: The number of eigenvalues/eigenvectors to compute.
  - `eigenvectors`: Output matrix to store the computed eigenvectors.
  - `solver_parameters`: Parameters to control the eigensolver's behavior.
  Calls the C wrapper: `EigenDecomposition_wrp`.

- **`NTPoly::EigenSolvers::EigenValues(const Matrix_ps &matrix, Matrix_ps &eigenvalues, int nvals, const SolverParameters &solver_parameters)`:**
  Computes a specified number (`nvals`) of eigenvalues for the input `matrix`, without calculating the eigenvectors.
  - `matrix`: The input matrix.
  - `eigenvalues`: Output matrix (typically a vector) to store the computed eigenvalues.
  - `nvals`: The number of eigenvalues to compute.
  - `solver_parameters`: Parameters to control the eigensolver's behavior.
  Calls the C wrapper: `EigenDecomposition_novec_wrp`.

- **`NTPoly::EigenSolvers::SingularValueDecomposition(const Matrix_ps &matrix, Matrix_ps &leftvectors, Matrix_ps &rightvectors, Matrix_ps &singularvalues, const SolverParameters &solver_parameters)`:**
  Performs Singular Value Decomposition (SVD) on the input `matrix`.
  - `matrix`: The input matrix for SVD.
  - `leftvectors`: Output matrix to store the left singular vectors.
  - `rightvectors`: Output matrix to store the right singular vectors.
  - `singularvalues`: Output matrix (typically a vector) to store the singular values.
  - `solver_parameters`: Parameters to control the SVD solver's behavior.
  Calls the C wrapper: `SingularValueDecompostion_wrp`.

- **`NTPoly::EigenSolvers::EstimateGap(const Matrix_ps &H, const Matrix_ps &K, double chemical_potential, double *gap, const SolverParameters &solver_parameters)`:**
  Estimates the HOMO-LUMO (Highest Occupied Molecular Orbital - Lowest Unoccupied Molecular Orbital) gap.
  - `H`: The Hamiltonian matrix.
  - `K`: Another matrix, potentially an overlap matrix or identity, depending on the specific method used by the underlying C routine.
  - `chemical_potential`: The chemical potential value.
  - `gap`: A pointer to a double where the estimated gap will be stored.
  - `solver_parameters`: Parameters to control the gap estimation algorithm.
  Calls the C wrapper: `EstimateGap_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants. The behavior of the functions is primarily determined by the input matrices and solver parameters.

## Usage Examples

No specific usage examples were found in the file. These solver functions are intended to be utilized by higher-level computational routines within the NTPoly library.

```cc
// No specific usage examples were found in the file.
// A conceptual usage might be:

// NTPoly::Matrix_ps A, eigvals, eigvecs, U, V, S_diag;
// // ... Initialize matrix A, and other matrices for output ...
// NTPoly::SolverParameters params;
// // ... Configure params ...
// int num_eigenvalues_to_find = 5;

// // Full Eigen Decomposition
// NTPoly::EigenSolvers::EigenDecomposition(A, eigvals, num_eigenvalues_to_find, eigvecs, params);

// // Eigenvalues only
// NTPoly::EigenSolvers::EigenValues(A, eigvals, num_eigenvalues_to_find, params);

// // Singular Value Decomposition
// NTPoly::EigenSolvers::SingularValueDecomposition(A, U, V, S_diag, params);

// // Gap Estimation
// NTPoly::Matrix_ps hamiltonian, overlap_matrix;
// // ... Initialize hamiltonian, overlap_matrix ...
// double mu = 0.0;
// double estimated_gap;
// NTPoly::EigenSolvers::EstimateGap(hamiltonian, overlap_matrix, mu, &estimated_gap, params);
```

## Dependencies and Interactions

- **Headers:**
  - `#include "EigenSolvers.h"`: Includes the C++ class declaration for `NTPoly::EigenSolvers`.
  - `#include "EigenSolvers_c.h"`: Includes declarations for the C wrapper functions (e.g., `EigenDecomposition_wrp`, `SingularValueDecompostion_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, representing input matrices, eigenvalues, eigenvectors, singular vectors, and singular values. The `GetIH()` method is used to pass their C handles to the C wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, which are passed to control the behavior of the underlying solvers, also using `GetIH()`.
- **External C Functions:** The core computational logic for all solver routines is delegated to external C functions (suffixed with `_wrp`). This file provides the C++ interface to these C implementations.
```
