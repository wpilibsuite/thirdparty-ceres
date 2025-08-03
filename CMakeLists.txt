if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "RelWithDebInfo" CACHE STRING "" FORCE)
endif()

cmake_minimum_required(VERSION 3.25)
set(CMAKE_OSX_ARCHITECTURES arm64 x86_64)
set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT Embedded)
project(thirdparty-ceres C CXX)
if(TOOLCHAIN_TRIPLE STREQUAL aarch64-linux-gnu)
    # Hack around opensdk not having the wrong arch and Ceres not being able to detect the Eigen version when on aarch64
    set(CMAKE_SYSTEM_PROCESSOR ARM64)
endif()
set(BUILD_SHARED_LIBS OFF)
set(BUILD_TESTING OFF)
set(BUILD_EXAMPLES OFF)

set(NO_CBLAS ON)
set(NO_LAPACKE ON)
set(C_LAPACK ON)
set(USE_LOCKING ON)
set(USE_THREAD OFF)
set(NOFORTRAN ON)

set(SUITESPARSE_ENABLE_PROJECTS cholmod)
set(SUITESPARSE_USE_CUDA OFF)
set(SUITESPARSE_USE_FORTRAN OFF)
set(SUITESPARSE_USE_OPENMP OFF)
set(SUITESPARSE_USE_PYTHON OFF)
set(CHOLMOD_GPL OFF)
if(NOT APPLE)
    set(BLA_VENDOR OpenBLAS)
endif()

set(EIGEN_BUILD_CMAKE_PACKAGE ON)

set(WITH_GFLAGS OFF)
set(WITH_GTEST OFF)

set(BUILD_BENCHMARKS OFF)
set(MINIGLOG ON)
set(PROVIDE_UNINSTALL_TARGET OFF)
set(SUITESPARSE OFF)
# We don't use the SPARSE_SCHUR solver, so the generated code is just file size bloat
set(SCHUR_SPECIALIZATIONS OFF)
include(FetchContent)
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/cmake)
if(NOT APPLE)
    fetchcontent_declare(
        BLAS
        GIT_REPOSITORY https://github.com/OpenMathLib/OpenBLAS.git
        GIT_TAG v0.3.30
        OVERRIDE_FIND_PACKAGE
    )
endif()

fetchcontent_declare(
    SuiteSparse
    GIT_REPOSITORY https://github.com/DrTimothyAldenDavis/SuiteSparse.git
    GIT_TAG v7.11.0
    OVERRIDE_FIND_PACKAGE
)

fetchcontent_declare(
    Eigen3
    GIT_REPOSITORY https://gitlab.com/libeigen/eigen.git
    GIT_TAG d81aa18f4dc56264b2cd7e2f230807d776a2d385
    OVERRIDE_FIND_PACKAGE 
)

fetchcontent_declare(
    Ceres
    GIT_REPOSITORY https://github.com/ceres-solver/ceres-solver.git
    GIT_TAG 2.2.0
)
if(NOT APPLE)
    fetchcontent_makeavailable(BLAS)
endif()
fetchcontent_makeavailable(SuiteSparse Eigen3 Ceres)
