if(NOT IS_MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -Wno-narrowing")
endif()

if(NOT SIMD_ENABLED)
    add_definitions(-DDISABLE_CPU_NEON)
    add_definitions(-DDISABLE_CPU_AVX)
    add_definitions(-DDISABLE_CPU_SSE)
endif()

if(CUDA_ENABLED)
    # Fix for Ubuntu 16.04.
    add_definitions("-D_FORCE_INLINES")

    set(PBA_GPU_SOURCE_FILES
        ConfigBA.cpp
        ConfigBA.h
        CuTexImage.cpp
        CuTexImage.h
        DataInterface.h
        pba.cpp
        pba.h
        ProgramCU.cu
        ProgramCU.h
        SparseBundleCPU.cpp
        SparseBundleCPU.h
        SparseBundleCU.cpp
        SparseBundleCU.h
        util.h)
    COLMAP_CUDA_ADD_LIBRARY(pba ${PBA_GPU_SOURCE_FILES})
else()
    set(PBA_CPU_SOURCE_FILES
        ConfigBA.cpp
        ConfigBA.h
        DataInterface.h
        pba.cpp
        pba.h
        SparseBundleCPU.cpp
        SparseBundleCPU.h
        util.h)
    COLMAP_ADD_LIBRARY(pba ${PBA_CPU_SOURCE_FILES})
    set_target_properties(pba PROPERTIES COMPILE_FLAGS "-DPBA_NO_GPU")
endif()
