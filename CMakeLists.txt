cmake_minimum_required(VERSION 3.20)
project(CoreQuarryWorkspace)

# ==============================================================================
# Hardware Acceleration & Optimization Flags 
# ==============================================================================

# Global optimization and position-independent code (crucial for shared libraries)
add_compile_options(-O3 -fPIC)

if(APPLE)
    # Force entire build tree to use Apple Silicon acceleration
    add_compile_definitions(
        GGML_USE_METAL=1
        GGML_USE_ACCELERATE=1
    )
endif()

if(CMAKE_CUDA_COMPILER OR DEFINED CUDAToolkit_FOUND)
    # Force entire build tree to use NVIDIA CUDA acceleration
    add_compile_definitions(
        GGML_USE_CUDA=1
    )
endif()

# ==============================================================================
# Dependency Resolution
# ==============================================================================

# Define the expected path to GGML at project root
set(GGML_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/ggml")

if(EXISTS "${GGML_SOURCE_DIR}/CMakeLists.txt")
    message(STATUS "Found GGML at base directory: ${GGML_SOURCE_DIR}")

    # GGML will now successfully inherit METAL, ACCELERATE, CUDA, and -O3/-fPIC
    add_subdirectory("${GGML_SOURCE_DIR}" "${CMAKE_CURRENT_BINARY_DIR}/ggml" EXCLUDE_FROM_ALL)
else()
    message(FATAL_ERROR "GGML not found at ${GGML_SOURCE_DIR}. Please clone GGML to the project base directory.")
endif()


# Add Schmate first so its targets (sbert_extended, bert_model) are defined
add_subdirectory(Schmate)
add_subdirectory(ib)
