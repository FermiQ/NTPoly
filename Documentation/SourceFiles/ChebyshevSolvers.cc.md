# `Source/CPlusPlus/ChebyshevSolvers.cc`

## Overview

This file provides a C++ wrapper for operations involving Chebyshev polynomials. It defines the `NTPoly::ChebyshevPolynomial` class, which allows for the creation, coefficient setting, and application of Chebyshev polynomials to matrices (`NTPoly::Matrix_ps`). The core computations are performed by underlying C routines, and this file acts as an interface layer to make these routines accessible from C++ in the NTPoly environment.

## Key Components

- **`NTPoly::ChebyshevPolynomial` (Class):** Represents a Chebyshev polynomial and provides methods for its manipulation and application.
  - **`ChebyshevPolynomial(int degree)`:** Constructor. Initializes a Chebyshev polynomial object with a specified maximum `degree`. It calls an underlying C wrapper `ConstructChebyshevPolynomial_wrp`.
  - **`~ChebyshevPolynomial()`:** Destructor. Releases resources associated with the Chebyshev polynomial object. It calls an underlying C wrapper `DestructChebyshevPolynomial_wrp`.
  - **`SetCoefficient(int degree, double coefficient)`:** Sets the `coefficient` for the term of the specified `degree` in the polynomial. Note that the `degree` parameter is internally incremented by 1 before being passed to the C wrapper `SetChebyshevCoefficient_wrp`.
  - **`Compute(const Matrix_ps &InputMat, Matrix_ps &OutputMat, const SolverParameters &solver_parameters) const`:** Applies the defined Chebyshev polynomial to the `InputMat` (i.e., computes P(InputMat)) and stores the result in `OutputMat`. It utilizes the C wrapper `ChebyshevCompute_wrp`.
  - **`ComputeFactorized(const Matrix_ps &InputMat, Matrix_ps &OutputMat, const SolverParameters &solver_parameters) const`:** Applies the defined Chebyshev polynomial to the `InputMat` using a factorized computation method. The result is stored in `OutputMat`. It utilizes the C wrapper `FactorizedChebyshevCompute_wrp`.

## Important Variables/Constants

- **`this->ih_this` (within `ChebyshevPolynomial` class):** This is an instance handle (likely a pointer or an integer type) used internally by each `ChebyshevPolynomial` object. It references the corresponding C data structure managed by the wrapper functions. It is consistently passed to the `_wrp` C functions to specify which polynomial object the operation applies to.

## Usage Examples

No specific usage examples were found in the file. The `ChebyshevPolynomial` class and its methods are intended to be used by other C++ components within the NTPoly library that require matrix functions based on Chebyshev polynomial expansions.

```cc
// No specific usage examples were found in the file.
// A conceptual usage would be:

// #include "ChebyshevSolvers.h"
// #include "Matrix.h" // Assuming Matrix_ps is defined here or in NTPoly.h
// #include "SolverParameters.h"

// // ... (Initialize ProcessGrid, Memory, etc. if needed) ...

// NTPoly::Matrix_ps my_matrix, result_matrix;
// // ... (Initialize my_matrix, result_matrix) ...

// NTPoly::SolverParameters params;
// // ... (Configure params if necessary) ...

// int poly_degree = 10;
// NTPoly::ChebyshevPolynomial cheby_poly(poly_degree);

// // Set some coefficients
// cheby_poly.SetCoefficient(0, 1.0); // T_0
// cheby_poly.SetCoefficient(2, -0.5); // T_2
// // ... set other coefficients as needed

// // Compute P(my_matrix)
// cheby_poly.Compute(my_matrix, result_matrix, params);

// // Or compute using factorized method
// // cheby_poly.ComputeFactorized(my_matrix, result_matrix, params);

// // ... (Use result_matrix) ...
```

## Dependencies and Interactions

- **Headers:**
  - `#include "ChebyshevSolvers.h"`: Includes the C++ class declaration for `NTPoly::ChebyshevPolynomial`.
  - `#include "SolverParameters.h"`: Includes the definition of `NTPoly::SolverParameters`, used by the compute methods.
  - `#include "ChebyshevSolvers_c.h"`: Includes declarations for the external C wrapper functions (e.g., `ConstructChebyshevPolynomial_wrp`, `ChebyshevCompute_wrp`).
- **Namespace:** All components are defined within the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, which represent the input and output matrices for polynomial computations. The `GetIH()` method is used to pass their C handles to the wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, which are passed to the compute methods to control solver behavior, also using `GetIH()`.
- **External C Functions:** The actual implementation of polynomial construction, destruction, coefficient setting, and computation is delegated to C functions (typically suffixed with `_wrp`). This file primarily serves as a C++ object-oriented interface to these C functions.
