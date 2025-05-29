# `Source/CPlusPlus/Polynomial.cc`

## Overview

This file provides a C++ wrapper for generic polynomial operations on matrices. It defines the `NTPoly::Polynomial` class, which allows users to define a polynomial by setting its coefficients and then apply this polynomial to an input matrix (`NTPoly::Matrix_ps`). Two methods are provided for computing the polynomial of a matrix: Horner's method and the Paterson-Stockmeyer algorithm. The actual computations are delegated to underlying C routines.

## Key Components

- **`NTPoly::Polynomial` (Class):**
  Represents a polynomial P(x) = c<sub>0</sub> + c<sub>1</sub>x + c<sub>2</sub>x<sup>2</sup> + ... + c<sub>n</sub>x<sup>n</sup> for matrix operations.
  - **`Polynomial(int degree)`:** Constructor. Initializes a polynomial object with a specified maximum `degree`.
    - Calls C wrapper: `ConstructPolynomial_wrp`.
  - **`~Polynomial()`:** Destructor. Releases resources associated with the polynomial object.
    - Calls C wrapper: `DestructPolynomial_wrp`.
  - **`SetCoefficient(int degree, double coefficient)`:** Sets the `coefficient` for the term of the specified `degree` in the polynomial. Note that the `degree` parameter is internally incremented by 1 before being passed to the C wrapper, likely to match a 1-based indexing scheme in the C backend.
    - Calls C wrapper: `SetCoefficient_wrp`.
  - **`HornerCompute(const Matrix_ps &InputMat, Matrix_ps &OutputMat, const SolverParameters &solver_parameters) const`:**
    Applies the defined polynomial to the `InputMat` (i.e., computes P(InputMat)) using Horner's method. The result is stored in `OutputMat`.
    - Calls C wrapper: `HornerCompute_wrp`.
  - **`PatersonStockmeyerCompute(const Matrix_ps &InputMat, Matrix_ps &OutputMat, const SolverParameters &solver_parameters) const`:**
    Applies the defined polynomial to the `InputMat` using the Paterson-Stockmeyer algorithm, which can be more efficient for higher-degree polynomials or specific matrix types. The result is stored in `OutputMat`.
    - Calls C wrapper: `PatersonStockmeyerCompute_wrp`.

## Important Variables/Constants

- **`this->ih_this` (within `Polynomial` class):** This is an instance handle (likely a pointer or an integer type) used internally by each `Polynomial` object. It references the corresponding C data structure managed by the wrapper functions.

## Usage Examples

No specific usage examples were found in the file. The `Polynomial` class and its methods are intended to be used by other C++ components within the NTPoly library that require applying arbitrary polynomial functions to matrices.

```cpp
// No specific usage examples were found in the file.
// A conceptual usage would be:

// #include "Polynomial.h"
// #include "PSMatrix.h" // Assuming Matrix_ps is defined here
// #include "SolverParameters.h"

// NTPoly::Matrix_ps my_matrix, result_matrix;
// // ... (Initialize my_matrix, result_matrix) ...

// NTPoly::SolverParameters params;
// // ... (Configure params if necessary) ...

// int poly_degree = 3;
// NTPoly::Polynomial p(poly_degree);

// // Define P(x) = 1 + 2x + 0.5x^2 + 3x^3
// p.SetCoefficient(0, 1.0);
// p.SetCoefficient(1, 2.0);
// p.SetCoefficient(2, 0.5);
// p.SetCoefficient(3, 3.0);

// // Compute P(my_matrix) using Horner's method
// p.HornerCompute(my_matrix, result_matrix, params);

// // Or using Paterson-Stockmeyer
// // p.PatersonStockmeyerCompute(my_matrix, result_matrix, params);

// // ... (Use result_matrix) ...
```

## Dependencies and Interactions

- **Headers:**
  - `#include "Polynomial.h"`: Includes the C++ class declaration for `NTPoly::Polynomial`.
  - `#include "SolverParameters.h"`: Includes the definition of `NTPoly::SolverParameters`, used by the compute methods.
  - `#include "Polynomial_c.h"`: Includes declarations for the external C wrapper functions (e.g., `ConstructPolynomial_wrp`, `HornerCompute_wrp`).
- **Namespace:** All components are defined within the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` objects, which represent the input and output matrices for polynomial computations. The `GetIH()` method is used to pass their C handles to the wrapper functions.
  - Interacts with `NTPoly::SolverParameters` objects, which are passed to the compute methods to control solver behavior, also using `GetIH()`.
- **External C Functions:** The actual implementation of polynomial construction, destruction, coefficient setting, and computation is delegated to C functions (typically suffixed with `_wrp`). This file primarily serves as a C++ object-oriented interface to these C functions.
```
