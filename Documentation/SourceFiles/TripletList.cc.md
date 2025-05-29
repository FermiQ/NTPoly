# `Source/CPlusPlus/TripletList.cc`

## Overview

This file implements the C++ wrapper classes `NTPoly::TripletList_r` and `NTPoly::TripletList_c`. These classes are designed to manage lists of "triplets," where each triplet typically represents a non-zero element of a sparse matrix using its row index, column index, and value. `TripletList_r` handles real-valued triplets, while `TripletList_c` handles complex-valued triplets. The classes provide functionalities for creating these lists, adding elements, accessing elements, resizing, and sorting. They serve as C++ interfaces to underlying C routines.

## Key Components

- **`NTPoly::TripletList_r` (Class):**
  Manages a dynamic list of real-valued triplets (`NTPoly::Triplet_r`). Each triplet consists of a row index, a column index, and a real value.
  - **`TripletList_r(int size = 0)`:** Constructor. Initializes an empty list, potentially with an initial reserved capacity specified by `size`.
    - Calls C wrapper: `ConstructTripletList_r_wrp`.
  - **`Resize(int size)`:** Resizes the internal storage of the triplet list.
    - Calls C wrapper: `ResizeTripletList_r_wrp`.
  - **`Append(const Triplet_r &value)`:** Adds a `Triplet_r` to the end of the list.
    - Calls C wrapper: `AppendToTripletList_r_wrp`.
  - **`SetTripletAt(int index, const Triplet_r &value)`:** Sets the triplet at the specified 0-based `index` to `value`. The index is adjusted to 1-based for the C wrapper.
    - Calls C wrapper: `SetTripletAt_r_wrp`.
  - **`GetTripletAt(int index) const`:** Returns the `Triplet_r` at the specified 0-based `index`. The index is adjusted to 1-based for the C wrapper.
    - Calls C wrapper: `GetTripletAt_r_wrp`.
  - **`GetSize() const`:** Returns the current number of triplets in the list.
    - Calls C wrapper: `GetTripletListSize_r_wrp`.
  - **`~TripletList_r()`:** Destructor. Releases resources associated with the list.
    - Calls C wrapper: `DestructTripletList_r_wrp`.
  - **`static SortTripletList(const TripletList_r &input, int matrix_columns, TripletList_r &sorted)`:** A static method to sort an `input` triplet list, producing a `sorted` list. `matrix_columns` might be used for certain sorting strategies (e.g., column-major).
    - Calls C wrapper: `SortTripletList_r_wrp`.

- **`NTPoly::TripletList_c` (Class):**
  Manages a dynamic list of complex-valued triplets (`NTPoly::Triplet_c`). Each triplet consists of a row index, a column index, and a `std::complex<double>` value. It mirrors the methods of `TripletList_r` but for complex data.
  - **`TripletList_c(int size = 0)`:** Constructor.
    - Calls C wrapper: `ConstructTripletList_c_wrp`.
  - **`Resize(int size)`:**
    - Calls C wrapper: `ResizeTripletList_c_wrp`.
  - **`Append(const Triplet_c &value)`:**
    - Calls C wrapper: `AppendToTripletList_c_wrp`.
  - **`SetTripletAt(int index, const Triplet_c &value)`:**
    - Calls C wrapper: `SetTripletAt_c_wrp`.
  - **`GetTripletAt(int index) const`:**
    - Calls C wrapper: `GetTripletAt_c_wrp`.
  - **`GetSize() const`:**
    - Calls C wrapper: `GetTripletListSize_c_wrp`.
  - **`~TripletList_c()`:** Destructor.
    - Calls C wrapper: `DestructTripletList_c_wrp`.
  - **`static SortTripletList(const TripletList_c &input, int matrix_columns, TripletList_c &sorted)`:** Static sort method for complex triplets.
    - Calls C wrapper: `SortTripletList_c_wrp`.

## Important Variables/Constants

- **`ih_this` (private member of `TripletList_r` and `TripletList_c`):** An internal instance handle (likely a pointer or integer type) that references the underlying C data structure for the triplet list. This handle is passed to the C wrapper functions.
- **`Triplet_r` / `Triplet_c` members (defined in `Triplet.h`):**
    - `index_column`: Integer column index of the matrix element.
    - `index_row`: Integer row index of the matrix element.
    - `point_value`: The actual value of the matrix element (double for `Triplet_r`, `std::complex<double>` for `Triplet_c`).

## Usage Examples

```cpp
// Conceptual C++ example for real triplets:
// #include "TripletList.h"
// #include "Triplet.h" // For NTPoly::Triplet_r definition

// NTPoly::TripletList_r myList; // Create an empty list
// myList.Append(NTPoly::Triplet_r(0, 0, 1.0)); // Add triplet (row 0, col 0, value 1.0)
// myList.Append(NTPoly::Triplet_r(1, 2, 5.5)); // Add triplet (row 1, col 2, value 5.5)

// if (myList.GetSize() > 0) {
//   NTPoly::Triplet_r first_element = myList.GetTripletAt(0);
//   // first_element.index_row, first_element.index_column, first_element.point_value
// }

// NTPoly::TripletList_r sortedList;
// int num_matrix_cols = 3; // Assuming the matrix these triplets belong to has 3 columns
// NTPoly::TripletList_r::SortTripletList(myList, num_matrix_cols, sortedList);

// // These lists are often used to construct sparse matrices:
// // NTPoly::Matrix_ps sparse_matrix;
// // sparse_matrix.FillFromTripletList(sortedList);
```

## Dependencies and Interactions

- **Headers:**
  - `#include "TripletList.h"`: Contains the C++ class declarations for `NTPoly::TripletList_r` and `NTPoly::TripletList_c`.
  - `#include "Triplet.h"`: Defines the `NTPoly::Triplet_r` and `NTPoly::Triplet_c` structures/classes that these lists manage.
  - `#include <complex>`: Standard C++ header for `std::complex<double>`.
  - C Header: `#include "TripletList_c.h"`: Includes declarations for the C wrapper functions that implement the core list operations.
- **Namespace:** All components are defined within the `NTPoly` namespace.
- **External C Functions:** All list operations (construction, appending, sorting, destruction, etc.) are delegated to C functions (typically suffixed with `_r_wrp` or `_c_wrp`). The C++ classes provide an object-oriented interface to these C routines.
- **Matrix Classes:** Triplet lists are fundamental for initializing or extracting data from sparse matrix classes in NTPoly, such as `Matrix_ps` (distributed sparse matrix) and `Matrix_lsr`/`Matrix_lsc` (local sparse matrices).
```
