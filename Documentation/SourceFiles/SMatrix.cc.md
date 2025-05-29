# `Source/CPlusPlus/SMatrix.cc`

## Overview

This file implements the C++ wrapper classes `NTPoly::Matrix_lsr` and `NTPoly::Matrix_lsc`. These classes represent local (non-distributed, sequential) sparse matrices for real (`_lsr`) and complex (`_lsc`) data types, respectively. They provide a comprehensive suite of methods for matrix construction (from dimensions, files, or triplet lists), destruction, querying attributes (rows, columns), data extraction (rows, columns, to triplet lists), various algebraic manipulations (scaling, incrementing, dot product, element-wise multiplication, GEMM, diagonal scaling, transpose, conjugate), and I/O operations (printing to console, writing to MatrixMarket files). These C++ classes serve as interfaces to an underlying C implementation.

## Key Components

- **`NTPoly::Matrix_lsr` (Class):** Represents a local sparse matrix of real numbers.
- **`NTPoly::Matrix_lsc` (Class):** Represents a local sparse matrix of complex numbers.

Both classes offer a similar set of methods tailored to their respective data types:

  - **Constructors:**
    - `Matrix_lsr(int columns, int rows)` / `Matrix_lsc(int columns, int rows)`: Creates a new zero-initialized matrix of the given dimensions.
    - `Matrix_lsr(std::string file_name)` / `Matrix_lsc(std::string file_name)`: Constructs a matrix by reading its content from a file (presumably in MatrixMarket format).
    - `Matrix_lsr(const TripletList_r &list, int rows, int columns)` / `Matrix_lsc(const TripletList_c &list, int rows, int columns)`: Constructs a matrix from a list of triplets, specifying the matrix dimensions.
    - `Matrix_lsr(const Matrix_lsr &matB)` / `Matrix_lsc(const Matrix_lsc &matB)`: Copy constructor.
  - **Destructor:** `~Matrix_lsr()` / `~Matrix_lsc()`: Releases resources associated with the matrix object.
  - **Dimension Queries:**
    - `GetRows() const`: Returns the number of rows in the matrix.
    - `GetColumns() const`: Returns the number of columns in the matrix.
  - **Data Extraction:**
    - `ExtractRow(int row_number, Matrix_lsr &row_out) const` / `ExtractRow(int row_number, Matrix_lsc &row_out) const`: Extracts a specified row into another matrix object.
    - `ExtractColumn(int column_number, Matrix_lsr &column_out) const` / `ExtractColumn(int column_number, Matrix_lsc &column_out) const`: Extracts a specified column into another matrix object.
    - `MatrixToTripletList(TripletList_r &triplet_list) const` / `MatrixToTripletList(TripletList_c &triplet_list) const`: Converts the matrix into a list of triplets.
  - **Matrix Operations:**
    - `Scale(double constant)`: Multiplies all elements of the matrix by a scalar.
    - `Increment(const Matrix_lsr &matB, double alpha, double threshold)` / `Increment(const Matrix_lsc &matB, double alpha, double threshold)`: Adds `alpha` times matrix `matB` to the current matrix.
    - `Dot(const Matrix_lsr &matB) const` / `Dot(const Matrix_lsc &matB) const`: Computes the dot product (Frobenius inner product) with matrix `matB`.
    - `PairwiseMultiply(const Matrix_lsr &matA, const Matrix_lsr &matB)` / `PairwiseMultiply(const Matrix_lsc &matA, const Matrix_lsc &matB)`: Performs element-wise multiplication of `matA` and `matB`, storing the result in the current matrix.
    - `Gemm(...)`: Performs general matrix multiplication (C = alpha*A*B + beta*C), taking transposition flags for A and B, and a memory pool.
    - `DiagonalScale(const TripletList_r &tlist)` / `DiagonalScale(const TripletList_c &tlist)`: Scales the matrix by a diagonal represented by triplets.
    - `Transpose(const Matrix_lsr &matA)` / `Transpose(const Matrix_lsc &matA)`: Computes the transpose of `matA` and stores it in the current matrix.
    - `Conjugate()` (for `Matrix_lsc` only): Computes the element-wise complex conjugate of the matrix.
  - **Output/Utility:**
    - `Print() const`: Prints the matrix content to standard output.
    - `WriteToMatrixMarket(string file_name) const`: Writes the matrix to a file in MatrixMarket format.

## Important Variables/Constants

- **`ih_this` (private member of `Matrix_lsr`/`Matrix_lsc`):** An internal instance handle (likely a pointer or integer type) that references the underlying C data structure for the local sparse matrix. This handle is passed to the C wrapper functions.

## Usage Examples

The methods provide a clear indication of typical usage patterns for a local sparse matrix library.

```cpp
// Conceptual C++ example for real matrices:
// #include "SMatrix.h"
// #include "TripletList.h"
// #include "MatrixMemoryPool.h" // For Gemm

// int r = 3, c = 3;
// NTPoly::Matrix_lsr A(c, r); // Create a 3x3 real matrix
// NTPoly::TripletList_r triplets_A;
// triplets_A.Append(NTPoly::Triplet_r(0,0,1.0));
// triplets_A.Append(NTPoly::Triplet_r(1,1,2.0));
// triplets_A.Append(NTPoly::Triplet_r(2,2,3.0));
// NTPoly::Matrix_lsr B(triplets_A, r, c); // B is now a diagonal matrix

// NTPoly::Matrix_lsr C(c, r);
// NTPoly::MatrixMemoryPool_r pool(c, r); // Memory pool for Gemm
// bool no_transpose = false;
// double alpha = 1.0, beta = 0.0, threshold = 1e-9;
// C.Gemm(B, B, no_transpose, no_transpose, alpha, beta, threshold, pool); // C = B*B

// C.Print();
// C.WriteToMatrixMarket("C_output.mtx");
```

## Dependencies and Interactions

- **Headers:**
  - `#include "SMatrix.h"`: Contains the C++ class declarations for `NTPoly::Matrix_lsr` and `NTPoly::Matrix_lsc`.
  - `#include "MatrixMemoryPool.h"`: Provides `NTPoly::MatrixMemoryPool_r` and `NTPoly::MatrixMemoryPool_c`, which are used as parameters in the `Gemm` methods.
  - `#include "TripletList.h"`: Provides `NTPoly::TripletList_r` and `NTPoly::TripletList_c` used for constructing matrices and for diagonal scaling.
  - C Header: `#include "SMatrix_c.h"`: Includes declarations for the numerous external C wrapper functions that implement the core logic for these local sparse matrices.
- **Standard Library:**
  - Uses `std::string` for file names.
  - Uses `std::complex` for complex number support in `Matrix_lsc`.
- **Namespace:** All components are defined within the `NTPoly` namespace.
- **External C Functions:** The majority of the functionality is delegated to C functions (typically suffixed with `_lsr_wrp` or `_lsc_wrp`). These C++ classes provide an object-oriented interface to the C backend.
```
