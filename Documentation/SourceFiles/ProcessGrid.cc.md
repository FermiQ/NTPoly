# `Source/CPlusPlus/ProcessGrid.cc`

## Overview

This file implements the `NTPoly::ProcessGrid` C++ class and a set of related global functions for managing and querying MPI (Message Passing Interface) process grids. A process grid defines the logical arrangement of MPI processes (e.g., into rows, columns, and slices) which is fundamental for distributing data and workload in parallel scientific computations, especially for distributed matrices. The class provides constructors to create process grid instances from MPI communicators and specified dimensions, while the global functions manage a default or primary process grid for the application. The implementation wraps underlying C routines.

## Key Components

- **`NTPoly::ProcessGrid` (Class):**
  Represents an instance of an MPI process grid.
  - **Constructors:**
    - `ProcessGrid(MPI_Comm world_comm, int process_rows, int process_columns, int process_slices)`: Creates a grid from a given MPI communicator and explicit row, column, and slice dimensions.
    - `ProcessGrid(int process_rows, int process_columns, int process_slices)`: Similar, but defaults to `MPI_COMM_WORLD`.
    - `ProcessGrid(MPI_Comm world_comm, int process_slices)`: Creates a grid, possibly inferring rows/columns or specializing for slice-based decomposition.
    - `ProcessGrid(int process_slices)`: Similar, using `MPI_COMM_WORLD`.
    - `ProcessGrid()`: Creates a default grid using `MPI_COMM_WORLD`.
    - `ProcessGrid(const ProcessGrid &old_grid)`: Copy constructor.
  - **Destructor:** `~ProcessGrid()`: Releases resources associated with the process grid instance.
  - **Query Methods:**
    - `GetMySlice()`: Returns the slice index of the current process in this grid.
    - `GetMyColumn()`: Returns the column index of the current process in this grid.
    - `GetMyRow()`: Returns the row index of the current process in this grid.
    - `GetNumSlices()`: Returns the total number of slices in this grid.
    - `GetNumColumns()`: Returns the total number of process columns in this grid.
    - `GetNumRows()`: Returns the total number of process rows in this grid.
  - **Utility Methods:**
    - `WriteInfo()`: Prints information about this process grid.

- **Global Process Grid Functions (within `NTPoly` namespace):**
  These functions operate on a globally accessible, default process grid.
  - `ConstructGlobalProcessGrid(...)`: Overloaded functions (similar to the class constructors) to initialize the global process grid.
  - `DestructGlobalProcessGrid()`: Releases resources of the global process grid.
  - `GetGlobalMySlice()`, `GetGlobalMyColumn()`, `GetGlobalMyRow()`: Query slice, column, row for the current process in the global grid.
  - `GetGlobalIsRoot()`: Checks if the current process is the root (typically rank 0) in the global grid.
  - `GetGlobalNumSlices()`, `GetGlobalNumColumns()`, `GetGlobalNumRows()`: Query dimensions of the global grid.
  - `WriteGridInfo()`: Prints information about the global process grid.

## Important Variables/Constants

- **`ih_this` (private member of `ProcessGrid` class):** An internal instance handle referencing the underlying C data structure for a process grid instance.
- **MPI Communicators:** `MPI_Comm` objects (e.g., `world_comm`, `MPI_COMM_WORLD`) are fundamental for defining the set of processes that form the grid. `MPI_Comm_c2f` is used to convert C MPI communicators to Fortran integers, suggesting interoperability with a Fortran backend.

## Usage Examples

```cpp
// Conceptual example for a ProcessGrid instance:
// #include "ProcessGrid.h" // Assuming this is the C++ header
// #include <mpi.h>

// int main(int argc, char* argv[]) {
//   MPI_Init(&argc, &argv);
//   MPI_Comm my_comm = MPI_COMM_WORLD;
//   int num_procs_rows = 2, num_procs_cols = 2, num_procs_slices = 1;

//   NTPoly::ProcessGrid grid(my_comm, num_procs_rows, num_procs_cols, num_procs_slices);
//   std::cout << "My process: Row " << grid.GetMyRow()
//             << ", Col " << grid.GetMyColumn()
//             << ", Slice " << grid.GetMySlice() << std::endl;
//   grid.WriteInfo();

//   // Conceptual example for the global grid:
//   // NTPoly::ConstructGlobalProcessGrid(num_procs_rows, num_procs_cols, num_procs_slices);
//   // if (NTPoly::GetGlobalIsRoot()) {
//   //   std::cout << "Global grid initialized by root." << std::endl;
//   // }
//   // NTPoly::WriteGridInfo();
//   // NTPoly::DestructGlobalProcessGrid();

//   MPI_Finalize();
//   return 0;
// }
```

## Dependencies and Interactions

- **Headers:**
  - `#include "ProcessGrid.h"`: Contains the C++ class declaration for `NTPoly::ProcessGrid`.
  - C Header: `#include "ProcessGrid_c.h"`: Includes declarations for the C wrapper functions that manage the process grid logic.
- **MPI:** Requires an MPI (Message Passing Interface) library for parallel processing.
- **Namespace:** The class and global functions are part of the `NTPoly` namespace.
- **External C Functions:** All core operations (construction, destruction, querying) are delegated to C functions (suffixed with `_wrp`). The C++ components provide an object-oriented and a global singleton-like interface to these C routines.
```
