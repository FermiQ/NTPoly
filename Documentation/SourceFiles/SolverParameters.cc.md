# `Source/CPlusPlus/SolverParameters.cc`

## Overview

This file implements the `NTPoly::SolverParameters` C++ class. This class acts as a container and interface for setting various parameters that control the behavior and precision of numerical solvers used throughout the NTPoly library. These parameters can include convergence criteria, iteration limits, thresholds for numerical operations, verbosity settings, and configurations for load balancing. The class wraps underlying C routines that manage these parameter values.

## Key Components

- **`NTPoly::SolverParameters` (Class):**
  Manages a set of parameters for controlling numerical solvers.
  - **`SolverParameters()`:** Constructor. Initializes a new solver parameters object, likely with default values set by the underlying C implementation.
    - Calls C wrapper: `ConstructSolverParameters_wrp`.
  - **`~SolverParameters()`:** Destructor. Releases resources associated with the solver parameters C object.
    - Calls C wrapper: `DestructSolverParameters_wrp`.
  - **Setter Methods:**
    - `SetConvergeDiff(double new_value)`: Sets the convergence criterion based on the difference between successive iterations.
      - Calls C wrapper: `SetParametersConvergeDiff_wrp`.
    - `SetMaxIterations(int new_value)`: Sets the maximum number of iterations a solver is allowed to perform.
      - Calls C wrapper: `SetParametersMaxIterations_wrp`.
    - `SetVerbosity(bool new_value)`: Sets a boolean flag to control whether solvers output detailed (verbose) information during execution.
      - Calls C wrapper: `SetParametersBeVerbose_wrp`.
    - `SetThreshold(double new_value)`: Sets a general numerical threshold, often used for operations like matrix element filtering or determining numerical zero.
      - Calls C wrapper: `SetParametersThreshold_wrp`.
    - `SetLoadBalance(const Permutation &new_value)`: Assigns a `NTPoly::Permutation` object to be used by solvers for load balancing purposes in parallel computations.
      - Calls C wrapper: `SetParametersLoadBalance_wrp`.
    - `SetStepThreshold(double new_value)`: Sets a threshold related to the step size or change in solution vectors in iterative methods.
      - Calls C wrapper: `SetParametersStepThreshold_wrp`.
    - `SetMonitorConvergence(bool new_value)`: Sets a boolean flag to enable or disable the monitoring of convergence during solver execution.
      - Calls C wrapper: `SetParametersMonitorConvergence_wrp`.

## Important Variables/Constants

- **`ih_this` (private member of `SolverParameters`):** An internal instance handle (likely a pointer or integer type) that references the underlying C data structure holding the actual parameter values. This handle is passed to the C wrapper functions.

## Usage Examples

The `SolverParameters` object is typically instantiated and configured before being passed to a solver function.

```cpp
// Conceptual C++ example:
// #include "SolverParameters.h"
// #include "Permutation.h" // If using SetLoadBalance
// #include "SomeSolverClass.h" // Example solver that uses these params

// NTPoly::SolverParameters params;
// params.SetConvergeDiff(1.0e-8);
// params.SetMaxIterations(1000);
// params.SetVerbosity(true);
// params.SetThreshold(1.0e-12);
// params.SetMonitorConvergence(true);

// // Example of setting load balancing if a permutation is defined
// // NTPoly::Permutation my_permutation(matrix_dim);
// // my_permutation.SetDefaultPermutation(); // or some other permutation
// // params.SetLoadBalance(my_permutation);

// // Pass params to a solver
// // SomeNTPolySolver solver;
// // solver.Solve(matrix_A, vector_b, solution_x, params);
```

## Dependencies and Interactions

- **Headers:**
  - `#include "SolverParameters.h"`: Contains the C++ class declaration for `NTPoly::SolverParameters`.
  - `#include "Permutation.h"`: Required for the `NTPoly::Permutation` type used in the `SetLoadBalance` method.
  - C Header: `#include "SolverParameters_c.h"`: Includes declarations for the C wrapper functions that get/set the parameter values in the C backend.
- **Namespace:** The class is part of the `NTPoly` namespace.
- **External C Functions:** All parameter manipulations are delegated to C functions (suffixed with `_wrp`). The C++ class provides an object-oriented interface for managing these settings.
- **Solvers:** Instances of `SolverParameters` are intended to be passed to various solver routines throughout the NTPoly library to configure their execution.
```
