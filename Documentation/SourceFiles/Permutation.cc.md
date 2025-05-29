# `Source/CPlusPlus/Permutation.cc`

## Overview

This file implements the `NTPoly::Permutation` C++ class, which serves as a wrapper for creating and managing permutation vectors. A permutation vector defines a reordering of elements. This class allows for the initialization of permutations as default (identity), reverse order, or random shuffles. It interfaces with underlying C routines for the actual construction and destruction of permutation data.

## Key Components

- **`NTPoly::Permutation` (Class):**
  Manages a permutation vector.
  - **`Permutation(int dimension)`:** Constructor. Initializes a permutation object for a specified `dimension`. The permutation data itself is not created by the constructor; a subsequent `Set...` method must be called.
  - **`SetDefaultPermutation()`:** Populates the permutation object to represent a default (identity) permutation (e.g., [0, 1, 2, ..., dimension-1]).
    - Calls C wrapper: `ConstructDefaultPermutation_wrp`.
  - **`SetReversePermutation()`:** Populates the permutation object to represent a reverse order permutation (e.g., [dimension-1, ..., 2, 1, 0]).
    - Calls C wrapper: `ConstructReversePermutation_wrp`.
  - **`SetRandomPermutation()`:** Populates the permutation object with a randomly shuffled sequence of indices from 0 to dimension-1.
    - Calls C wrapper: `ConstructRandomPermutation_wrp`.
  - **`~Permutation()`:** Destructor. If the permutation data has been filled by one of the `Set...` methods, it calls the C wrapper `DestructPermutation_wrp` to release the associated C resources.

## Important Variables/Constants

- **`ih_this` (private member of `Permutation`):** An internal instance handle (likely a pointer or integer type) that references the underlying C data structure for the permutation. This handle is used by the C wrapper functions.
- **`matrix_dimension` (private member of `Permutation`):** Stores the dimension (size) of the permutation vector.
- **`was_filled` (private member of `Permutation`):** A boolean flag indicating whether the permutation data has been initialized through one of the `Set...Permutation()` methods. This prevents calling the destructor on an uninitialized C object.

## Usage Examples

```cpp
// Conceptual example:

// int size = 10;
// NTPoly::Permutation p_identity(size);
// p_identity.SetDefaultPermutation(); // p_identity now holds [0, 1, ..., 9]

// NTPoly::Permutation p_random(size);
// p_random.SetRandomPermutation(); // p_random now holds a random shuffle of [0, ..., 9]

// // The NTPoly::Permutation objects will automatically clean up their C resources
// // when they go out of scope due to the destructor ~Permutation().
```

## Dependencies and Interactions

- **Headers:**
  - `#include "Permutation.h"`: Contains the C++ class declaration for `NTPoly::Permutation`.
  - C Header: `#include "Permutation_c.h"`: Includes declarations for the C wrapper functions used to construct and destruct the permutation data.
- **Namespace:** The class is part of the `NTPoly` namespace.
- **External C Functions:** The core operations of creating and destroying the permutation data are delegated to C functions (suffixed with `_wrp`). The C++ class manages the lifetime and provides a more object-oriented interface to these C routines.
```
