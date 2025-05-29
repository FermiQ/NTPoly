# `Source/CPlusPlus/HermiteSolvers.cc`

## Overview

This file provides a C++ wrapper for operations involving Hermite polynomials. It defines the `NTPoly::HermitePolynomial` class, which allows for the creation, coefficient setting, and application of Hermite polynomials to matrices (`NTPoly::Matrix_ps`). The core computations are performed by underlying C routines, and this file acts as an interface layer to make these routines accessible from C++ in the NTPoly environment.

## Key Components

- **`NTPoly::HermitePolynomial` (Class):** Represents a Hermite polynomial and provides methods for its manipulation and application.
  - **`HermitePolynomial(int degree)`:** Constructor. Initializes a Hermite polynomial object with a specified maximum `degree`. It calls an underlying C wrapper `ConstructHermitePolynomial_wrp`.
  - **`~HermitePolynomial()`:** Destructor. Releases resources associated with the Hermite polynomial object. It calls an underlying C wrapper `DestructHermitePolynomial_wrp`.
  - **`SetCoefficient(int degree, double coefficient)`:** Sets the `coefficient` for the term of the specified `degree` in the polynomial. Note that the `degree` parameter is internally incremented by 1 before being passed to the C wrapper `SetHermiteCoefficient_wrp`.
  - **`Compute(const Matrix_ps &InputMat, Matrix_ps &OutputMat, const SolverParameters &solver_parameters) const`:** Applies the defined Hermite polynomial to the `InputMat` (i.e., computes P(InputMat)) and stores the result in `OutputMat`. It utilizes the C wrapper `HermiteCompute_wrp`.

## Important Variables/Constants

- **`this->ih_this` (within `HermitePolynomial` class):** This is an instance handle (likely a pointer or an integer type) used internally by each `HermitePolynomial` object. It references the corresponding C data structure managed by the wrapper functions. It is consistently passed to the `_wrp` C functions to specify which polynomial object the operation applies to.

## Usage Examples

No specific usage examples were found in the file. The `HermitePolynomial` class and its methods are intended to be used by other C++ components within the NTPoly library that require matrix functions based on Hermite polynomial expansions.

```cc
// No specific usage examples were found in the file.
// A conceptual usage would be:

// #include "HermiteSolvers.h"
// #include "Matrix.h" // Assuming Matrix_ps is defined here or in NTPoly.h
// #include "SolverParameters.h"

// NTPoly::Matrix_ps my_matrix, result_matrix;
// // ... (Initialize my_matrix, result_matrix) ...

// NTPoly::SolverParameters params;
// // ... (Configure params if necessary) ...

// int poly_degree = 10;
// NTPoly::HermitePolynomial hermite_poly(poly_degree);

// // Set some coefficients
// hermite_poly.SetCoefficient(0, 1.0); // H_0
// hermite_poly.SetCoefficient(1, 2.0); // H_1
// // ... set other coefficients as needed

// // Compute P(my_matrix)
// hermite_poly.Compute(my_matrix, result_matrix, params);

// // ... (Use result_matrix) ...
```

## Dependencies and Interactions

- **Headers:**
  - `#include "HermiteSolvers.h"`: Includes the C++ class declaration for `NTPoly::HermitePolynomial`.
  - `#include "SolverParameters.h"`: Includes the definition of `NTPoly::SolverParameters`, used by the compute method.
  - `#include "HermiteSolvers_c.h"`: Includes declarations for the external C wrapper functions (e.g., `ConstructHermitePolynomial_wrp`, `HermiteCompute_wrp`).
- **Namespace:** All components are defined within the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, which represent the input and output matrices for polynomial computations. The `GetIH()` method is used to pass their C handles to the wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, which are passed to the compute method to control solver behavior, also using `GetIH()`.
- **External C Functions:** The actual implementation of polynomial construction, destruction, coefficient setting, and computation is delegated to C functions (typically suffixed with `_wrp`). This file primarily serves as a C++ object-oriented interface to these C functions.
```
