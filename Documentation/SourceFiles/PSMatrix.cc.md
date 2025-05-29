# `Source/CPlusPlus/PSMatrix.cc`

## Overview

This file implements the `NTPoly::Matrix_ps` class, which represents a distributed sparse matrix. This class is likely a cornerstone of the NTPoly library, providing a wide array of functionalities for matrix creation, manipulation, input/output, and algebraic operations. Most of its methods serve as C++ wrappers that call underlying C routines to perform the actual computations, enabling high-performance parallel matrix operations.

## Key Components

- **`NTPoly::Matrix_ps` (Class):**
  A class representing a distributed sparse matrix.
  - **Constructors:**
    - `Matrix_ps(int matrix_dimension)`: Creates an empty matrix of a given dimension.
    - `Matrix_ps(int matrix_dimension, const ProcessGrid &grid)`: Creates an empty matrix of a given dimension, distributed according to the specified `ProcessGrid`.
    - `Matrix_ps(std::string file_name, bool is_binary)`: Constructs a matrix by reading from a file (MatrixMarket text format or binary).
    - `Matrix_ps(std::string file_name, const ProcessGrid &grid, bool is_binary)`: Constructs a matrix from a file, distributing it according to the specified `ProcessGrid`.
    - `Matrix_ps(const Matrix_ps &matB)`: Copy constructor.
  - **Destructor:** `~Matrix_ps()`: Releases resources associated with the matrix.
  - **Input/Output:**
    - `WriteToBinary(std::string file_name) const`: Writes the matrix to a file in binary format.
    - `WriteToMatrixMarket(string file_name) const`: Writes the matrix to a file in MatrixMarket text format.
  - **Fill Operations:**
    - `FillFromTripletList(const TripletList_r &triplet_list)`: Fills the matrix from a list of real-valued triplets.
    - `FillFromTripletList(const TripletList_c &triplet_list)`: Fills the matrix from a list of complex-valued triplets.
    - `FillDistributedPermutation(const Permutation &lb, bool permuterows)`: Fills the matrix as a permutation matrix based on a `Permutation` object.
    - `FillIdentity()`: Fills the matrix as an identity matrix.
    - `FillDense()`: Fills the matrix as a dense matrix (details of values not specified, could be ones or random).
  - **Query Operations:**
    - `GetLogicalDimension() const`: Returns the logical dimension of the matrix.
    - `GetActualDimension() const`: Returns the actual (possibly padded or internal) dimension.
    - `GetSize() const`: Returns the number of non-zero elements (or total elements if dense).
    - `GetTripletList(TripletList_r &triplet_list) const`: Extracts non-zero elements as a list of real triplets.
    - `GetTripletList(TripletList_c &triplet_list) const`: Extracts non-zero elements as a list of complex triplets.
    - `GetMatrixBlock(TripletList_r &triplet_list, int start_row, int end_row, int start_column, int end_column)`: Extracts a block of the matrix into a real triplet list.
    - `GetMatrixBlock(TripletList_c &triplet_list, int start_row, int end_row, int start_column, int end_column)`: Extracts a block of the matrix into a complex triplet list.
    - `GetMatrixSlice(Matrix_ps &submatrix, int start_row, int end_row, int start_column, int end_column)`: Extracts a slice of the matrix into another `Matrix_ps` object.
    - `IsIdentity() const`: Checks if the matrix is an identity matrix.
  - **Matrix Manipulations & Algebra:**
    - `Transpose(const Matrix_ps &matA)`: Computes the transpose of `matA` and stores it in the current matrix.
    - `Conjugate()`: Conjugates the elements of the current matrix (if complex).
    - `Resize(int new_size)`: Resizes the matrix.
    - `Dot(const Matrix_ps &matB)`: Computes the dot product (Frobenius inner product) with matrix `matB` (real).
    - `Dot_c(const Matrix_ps &matB)`: Computes the dot product with matrix `matB` (complex).
    - `Increment(const Matrix_ps &matB, double alpha, double threshold)`: Increments the current matrix by `alpha` times `matB`.
    - `PairwiseMultiply(const Matrix_ps &matA, const Matrix_ps &matB)`: Performs element-wise multiplication of `matA` and `matB`, storing the result in the current matrix.
    - `Gemm(const Matrix_ps &matA, const Matrix_ps &matB, PMatrixMemoryPool &memory_pool, double alpha, double beta, double threshold)`: Performs general matrix multiplication (C = alpha*A*B + beta*C).
    - `Scale(double constant)`: Multiplies all elements of the matrix by a scalar `constant`.
    - `DiagonalScale(const TripletList_r &tlist)`: Scales the matrix by a diagonal represented by real triplets.
    - `DiagonalScale(const TripletList_c &tlist)`: Scales the matrix by a diagonal represented by complex triplets.
    - `Norm() const`: Computes the norm of the matrix (typically Frobenius norm).
    - `Trace() const`: Computes the trace of the matrix.
    - `MeasureAsymmetry() const`: Computes a measure of the matrix's asymmetry.
    - `Symmetrize()`: Symmetrizes the matrix.

## Important Variables/Constants

- **`ih_this` (private member of `Matrix_ps`):** This is an internal instance handle (likely a pointer or an integer type) that references the underlying C data structure for the distributed matrix. It is passed to almost all `_wrp` C functions.

## Usage Examples

The extensive list of methods demonstrates typical usage patterns for matrix libraries.

```cpp
// Conceptual example:
// NTPoly::ProcessGrid grid(num_rows_procs, num_cols_procs);
// NTPoly::Matrix_ps A(N, grid); // Create an N x N matrix distributed on grid
// A.FillIdentity();

// NTPoly::Matrix_ps B;
// B.ReadFromMatrixMarket("input.mtx", grid); // Read from file

// NTPoly::Matrix_ps C(N, grid);
// NTPoly::PMatrixMemoryPool pool(A); // Create a memory pool
// double alpha = 1.0, beta = 0.0, threshold = 1e-9;
// C.Gemm(A, B, pool, alpha, beta, threshold); // C = A*B

// double tr_C = C.Trace();
// C.WriteToBinary("output_C.bin");
```

## Dependencies and Interactions

- **Headers:**
  - `#include "PSMatrix.h"`: Contains the C++ class declaration for `NTPoly::Matrix_ps`.
  - `#include "PMatrixMemoryPool.h"`: For `NTPoly::PMatrixMemoryPool`, used in `Gemm`.
  - `#include "Permutation.h"`: For `NTPoly::Permutation`, used in `FillDistributedPermutation`.
  - `#include "ProcessGrid.h"`: For `NTPoly::ProcessGrid`, used in constructors.
  - `#include "TripletList.h"`: For `NTPoly::TripletList_r` and `NTPoly::TripletList_c`, used in fill and get operations.
  - C Header: `#include "PSMatrix_c.h"`: Includes declarations for the numerous external C wrapper functions that implement the core logic.
- **Standard Library:**
  - Uses `std::string` for file names.
  - Uses `std::complex` for complex number support.
  - Uses `std::iostream` (implicitly, through other headers or for potential debugging not visible in the .cc).
- **Namespace:** All components are defined within the `NTPoly` namespace.
- **External C Functions:** The vast majority of the functionality is delegated to C functions (typically suffixed with `_ps_wrp`, `_psr_wrp`, or `_psc_wrp`), indicating this class is a comprehensive C++ interface to a C backend for distributed matrix computations.
```
