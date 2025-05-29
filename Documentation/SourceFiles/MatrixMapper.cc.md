# `Source/CPlusPlus/MatrixMapper.cc`

## Overview

This file implements a "matrix mapper" functionality within the NTPoly library. Its purpose is to apply a user-defined operation to each element (or triplet of row, column, value) of a matrix. The operations can be for real or complex matrices. The mapping process can be performed in parallel by dividing the matrix elements into slices, with each process handling its assigned slice. The results are collected into an output matrix. This is a flexible way to perform custom element-wise transformations or filtering on matrices.

## Key Components

The primary components are static methods of the `NTPoly::MatrixMapper` class and the associated operation functors (`RealOperation`, `ComplexOperation`, which are expected to be defined in `MatrixMapper.h`).

- **`NTPoly::MatrixMapper::GetSliceInfo(const Matrix_ps &mat, int &num_slices, int &my_slice)`:**
  Retrieves information about how a distributed matrix `mat` is sliced across processes.
  - `mat`: The input distributed matrix.
  - `num_slices`: Output; the total number of slices the matrix is divided into.
  - `my_slice`: Output; the index of the slice managed by the current process.
  - This function calls underlying C wrappers: `GetMatrixProcessGrid_ps_wrp`, `GetNumSlices_wrp`, `GetMySlice_wrp`.

- **`NTPoly::MatrixMapper::Map(const Matrix_ps &inmat, Matrix_ps &outmat, RealOperation *proc)`:**
  Applies a real-valued operation `proc` to each element of `inmat` and stores the results in `outmat`. It first converts `inmat` to a real triplet list.
  - `inmat`: The input real matrix.
  - `outmat`: The output real matrix.
  - `proc`: A pointer to a `RealOperation` functor that defines the operation to be applied to each triplet.

- **`NTPoly::MatrixMapper::Map(const Matrix_ps &inmat, Matrix_ps &outmat, ComplexOperation *proc)`:**
  Similar to the real version, but for complex matrices and `ComplexOperation` functors.
  - `inmat`: The input complex matrix.
  - `outmat`: The output complex matrix.
  - `proc`: A pointer to a `ComplexOperation` functor.

- **`NTPoly::MatrixMapper::Map(const TripletList_r &inlist, TripletList_r &outlist, RealOperation *proc, int num_slices, int my_slice)`:**
  The core logic for applying a real operation. It iterates over the triplets in `inlist` that belong to `my_slice`, applies `proc` to each, and appends the triplet to `outlist` if `proc` indicates it's valid (by returning true from its `operator()`).
  - `inlist`: Input list of real triplets.
  - `outlist`: Output list of real triplets.
  - `proc`: The real operation functor.
  - `num_slices`, `my_slice`: Slicing information for parallel processing.

- **`NTPoly::MatrixMapper::Map(const TripletList_c &inlist, TripletList_c &outlist, ComplexOperation *proc, int num_slices, int my_slice)`:**
  The core logic for applying a complex operation, analogous to the real version.

- **`RealOperation` / `ComplexOperation` (Assumed defined in `MatrixMapper.h`):**
  These are base classes or structs that users must inherit from to define their custom operations. They typically have a `data` member (a `Triplet_r` or `Triplet_c`) and an overloaded `operator()` that performs the logic and returns `true` if the (potentially modified) triplet should be included in the output.

## Important Variables/Constants

- **`proc->data` (within `RealOperation` or `ComplexOperation`):** This member variable within the user-defined operation functor holds the current triplet (row, column, value) being processed. The functor's logic reads from and can modify this triplet.

## Usage Examples

No specific usage examples of calling these `Map` functions are provided directly in `MatrixMapper.cc`. To use this functionality, a developer would typically:
1. Define a new class that inherits from `NTPoly::RealOperation` or `NTPoly::ComplexOperation`.
2. Implement the `operator()` within this new class to perform the desired transformation or filtering on the `this->data` triplet.
3. Instantiate this custom operation class.
4. Call one of the `NTPoly::MatrixMapper::Map` functions with the input matrix, an output matrix, and a pointer to the custom operation instance.

```cpp
// Conceptual example (RealOperation and ComplexOperation would be defined in MatrixMapper.h)

// // User-defined operation: Multiply value by 2 if row == column
// class MyRealOp : public NTPoly::RealOperation {
// public:
//   bool operator()() override { // proc() in the C++ code is actually proc->operator()
//     if (this->data.row == this->data.column) {
//       this->data.value *= 2.0;
//       return true; // Keep this triplet
//     }
//     return false; // Discard this triplet
//   }
// };

// NTPoly::Matrix_ps input_matrix, output_matrix;
// // ... Initialize input_matrix ...
// MyRealOp my_op_instance;
// NTPoly::MatrixMapper::Map(input_matrix, output_matrix, &my_op_instance);
// // output_matrix now contains the transformed elements.
```

## Dependencies and Interactions

- **Headers:**
  - `#include "MatrixMapper.h"`: Contains the declaration of `NTPoly::MatrixMapper` and likely `NTPoly::RealOperation` and `NTPoly::ComplexOperation`.
  - `#include "PSMatrix.h"`: For `NTPoly::Matrix_ps` and its methods like `GetTripletList` and `FillFromTripletList`.
  - `#include "Triplet.h"`: Defines `NTPoly::Triplet_r` and `NTPoly::Triplet_c`.
  - `#include "TripletList.h"`: Defines `NTPoly::TripletList_r` and `NTPoly::TripletList_c`.
  - `#include "Wrapper.h"`: For constants like `SIZE_wrp`.
  - C Headers: `PSMatrix_c.h` and `ProcessGrid_c.h` for functions related to distributed matrix properties.
- **Standard Library:**
  - Uses `std::complex` for complex number operations.
- **Namespace:** All components are part of the `NTPoly` namespace.
- **External C Functions:** Relies on C functions (e.g., `GetMatrixProcessGrid_ps_wrp`, `GetNumSlices_wrp`) for information about the distribution of matrices.
- **User Implementation:** The functionality heavily relies on the user providing a concrete implementation of `RealOperation` or `ComplexOperation`.
```
