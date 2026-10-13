# Algorithm Optimisation, Cache Behaviour and Concurrency

This repository contains the C++ implementations and experiments developed for my master's thesis. The work explores algorithm optimisation, data structures, memory-access patterns, cache locality, and—where relevant—concurrency.

The aim is to make each experiment straightforward to build, run, measure, and reproduce. Because performance depends on the hardware, operating system, compiler, build configuration, and input data, these details should be recorded with every set of results.

## Research scope

Experiments will be added incrementally as the thesis progresses. Each experiment will document its research question, implementation variants, test inputs, measurement method, results, and conclusions.

The first experiment is matrix multiplication, beginning with row-wise and column-wise traversal to investigate how memory-access patterns affect performance.

## Repository structure

```text
master-thesis/
├── CMakeLists.txt
└── experiments/
    └── 01-matrix-multiplication/
        ├── CMakeLists.txt
        └── main.cpp
```

- The root `CMakeLists.txt` configures the overall project and registers the experiments.
- Each experiment has its own directory and `CMakeLists.txt` so it can be built and extended independently.
- Build output and generated files belong in the build directory; they should not be committed to the repository.

## Requirements

- Git
- CMake 3.20 or newer
- A C++20-compatible compiler, such as GCC, Clang, or MSVC

Install a compiler and CMake using the tools appropriate for your operating system. For example, on Ubuntu or Ubuntu under WSL:

```bash
sudo apt update
sudo apt install build-essential cmake git
```

Check that the tools are available:

```bash
g++ --version
cmake --version
git --version
```

A different compiler can be used if it is supported by CMake and the project's C++ standard requirements.

## Get the source code

Clone the repository and enter its root directory (the directory containing the root `CMakeLists.txt`):

```bash
git clone https://github.com/aryan2574/master-thesis.git
cd master-thesis
```

## Configure and build

CMake is used to configure and build the experiments. Run these commands from the repository root.

For single-configuration generators commonly used with GCC or Clang on Linux and macOS:

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --parallel
```

For multi-configuration generators, such as the Visual Studio generator on Windows:

```powershell
cmake -S . -B build
cmake --build build --config Release --parallel
```

To build only the matrix multiplication target:

```bash
cmake --build build --target matrix_multiplication --config Release --parallel
```

The `--config Release` argument is used by multi-configuration generators and is harmless for common single-configuration generators. For single-configuration builds, set `CMAKE_BUILD_TYPE=Release` during configuration as shown above.

## Run an experiment

After a successful build, run the generated `matrix_multiplication` executable. Its exact location depends on the operating system, compiler, and CMake generator. For a typical single-configuration Linux build, run:

```bash
./build/experiments/01-matrix-multiplication/matrix_multiplication
```

For a typical Visual Studio multi-configuration build on Windows, the executable is usually under the experiment's `Release` build directory, for example:

```powershell
.\build\experiments\01-matrix-multiplication\Release\matrix_multiplication.exe
```

If the executable is not at that exact path, inspect the generated `build` directory or use your IDE's run configuration for the `matrix_multiplication` CMake target.

You can also open the repository root in any IDE that supports CMake projects. Load or configure the root `CMakeLists.txt`, choose an available C++ toolchain, and select the target for the experiment you want to run. The project does not require a specific IDE.

## Benchmarking and reproducibility

For a fair comparison, keep the compiler, optimisation settings, inputs, and measurement procedure consistent across implementations. Use a Release build for performance measurements, repeat measurements, and avoid drawing conclusions from a single run. Verify that compared implementations produce equivalent results.

Record at least the following for each experiment:

- CPU model and operating system
- Compiler name and version
- Build configuration and compiler flags
- Input sizes and implementation variant
- Number of repetitions and the summary statistic reported
- Timing method and any profiling tools or hardware counters used

Where supported, collect hardware-counter data—such as cache misses—alongside elapsed time. Be aware that virtualised environments and some operating systems may limit access to hardware performance counters.

Do not compare Debug-build timings with Release-build timings. Document environmental limitations and any measurement conditions that could affect the results.