# `Source/CPlusPlus/LoadBalancer.cc`

## Overview

This file provides C++ wrapper functions for operations related to load balancing of distributed matrices. Specifically, it offers functionalities to permute a matrix based on a defined permutation scheme and to undo such a permutation. These operations are crucial in high-performance parallel computing to ensure efficient data distribution and computation across processors. The functions interface with underlying C routines and utilize `NTPoly::Matrix_ps`, `NTPoly::Permutation`, and `NTPoly::PMatrixMemoryPool` objects.

## Key Components

All functions are static members of the `NTPoly::LoadBalancer` class, which is expected to be declared in `LoadBalancer.h`.

- **`NTPoly::LoadBalancer::PermuteMatrix(const Matrix_ps &mat_in, Matrix_ps &mat_out, const Permutation &permutation, PMatrixMemoryPool &memorypool)`:**
  Applies a permutation to the input matrix `mat_in` according to the rules defined in the `permutation` object. The resulting permuted matrix is stored in `mat_out`. This operation typically involves data redistribution and requires a `PMatrixMemoryPool` for managing distributed memory resources.
  - `mat_in`: The input distributed matrix to be permuted.
  - `mat_out`: The output matrix to store the permuted result.
  - `permutation`: An object defining the permutation to be applied.
  - `memorypool`: A distributed matrix memory pool for allocating `mat_out`.
  - Calls the C wrapper: `PermuteMatrix_wrp`.

- **`NTPoly::LoadBalancer::UndoPermuteMatrix(const Matrix_ps &mat_in, Matrix_ps &mat_out, const Permutation &permutation, PMatrixMemoryPool &memorypool)`:**
  Reverses or undoes a permutation that was previously applied to `mat_in` using the same `permutation` object. The unpermuted (original order) matrix is stored in `mat_out`.
  - `mat_in`: The input distributed matrix that has been permuted.
  - `mat_out`: The output matrix to store the matrix in its original, unpermuted order.
  - `permutation`: The object defining the permutation that was originally applied (and is now to be undone).
  - `memorypool`: A distributed matrix memory pool for allocating `mat_out`.
  - Calls the C wrapper: `UndoPermuteMatrix_wrp`.

## Important Variables/Constants

This file does not define any standalone global variables or constants. The core elements are the matrix objects, the permutation object, and the memory pool used for managing distributed data.

## Usage Examples

No specific usage examples were found in the file. These functions are typically used within larger parallel algorithms that require dynamic data redistribution for load balancing or optimizing communication patterns.

```cpp
// No specific usage examples were found in the file.
// A conceptual usage might be:

// NTPoly::Matrix_ps original_matrix, permuted_matrix, unpermuted_matrix;
// NTPoly::Permutation distribution_permutation;
// NTPoly::PMatrixMemoryPool distributed_pool;
// // ... Initialize original_matrix, distribution_permutation, and distributed_pool ...

// // Apply a permutation for load balancing
// NTPoly::LoadBalancer::PermuteMatrix(original_matrix, permuted_matrix,
//                                    distribution_permutation, distributed_pool);

// // ... Perform computations using permuted_matrix ...

// // Undo the permutation to restore original data layout if needed
// NTPoly::LoadBalancer::UndoPermuteMatrix(permuted_matrix, unpermuted_matrix,
//                                        distribution_permutation, distributed_pool);
```

## Dependencies and Interactions

- **Headers:**
  - `#include "LoadBalancer.h"`: Includes the C++ class declaration for `NTPoly::LoadBalancer`.
  - `#include "PMatrixMemoryPool.h"`: Includes the definition for `NTPoly::PMatrixMemoryPool`, essential for distributed matrix memory management.
  - `#include "PSMatrix.h"`: Includes the definition for `NTPoly::Matrix_ps` (or `PSMatrix`), the distributed matrix type.
  - `#include "Permutation.h"`: Includes the definition for `NTPoly::Permutation`, which specifies how matrix elements are reordered.
  - `#include "LoadBalancer_c.h"`: Includes declarations for the C wrapper functions (`PermuteMatrix_wrp`, `UndoPermuteMatrix_wrp`).
- **Namespace:** All functions are part of the `NTPoly` namespace.
- **Objects:**
  - Interacts with `NTPoly::Matrix_ps` (distributed matrices), `NTPoly::Permutation` (permutation rules), and `NTPoly::PMatrixMemoryPool` (memory management for distributed matrices). The internal handles (`ih_this`) of these objects are passed to the C wrapper functions.
- **External C Functions:** The core logic for permuting matrices is delegated to external C functions (suffixed with `_wrp`). This file provides the C++ interface.
```
