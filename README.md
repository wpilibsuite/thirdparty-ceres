# External Ceres repo

This is the repo that builds ceres and its dependents for for use in wpical. It uses CMake to build all the dependencies and Gradle to package it for use in allwpilib.

# Getting Started

The build requirements are the same as [allwpilib](https://github.com/wpilibsuite/allwpilib/?tab=readme-ov-file#requirements).

1. Clone the repo with `git clone --recursive https://github.com/wpilibsuite/thirdparty-ceres.git`.
2. Build the artifacts with `gradlew copyAllOutputs` and publish the artifacts locally with `gradlew publish`.

# Notes

The Eigen version must be synchronized with WPILib's, or compatibility issues may arise. This includes patches made via upstream_utils that change Eigen functionality. As of 2025.2.1, this repo copied one patch used to make assignment constexpr, but did not copy over a patch to disable warnings or a patch to fix Intellisense, since neither affect the binary output.
