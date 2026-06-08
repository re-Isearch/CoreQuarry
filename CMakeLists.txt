cmake_minimum_required(VERSION 3.20)
project(CoreQuarryWorkspace)

if(APPLE)
    # Force BOTH libraries to see the exact same GGML configuration
    add_compile_definitions(
        GGML_USE_METAL=1
        GGML_USE_ACCELERATE=1
    )

    add_compile_options(-O3 -fPIC)
endif()

if(CMAKE_CUDA_COMPILER OR DEFINED CUDAToolkit_FOUND)
    # Force BOTH libraries to see the exact same GGML configuration
    add_compile_definitions(
        GGML_USE_CUDA=1
    )

    add_compile_options(-O3 -fPIC)
endif()



# Add Schmate first so its targets (sbert_extended, bert_model) are defined
add_subdirectory(Schmate)
add_subdirectory(ib)
