cmake_minimum_required(VERSION 3.10)

add_library(CMP_Framework STATIC "")

if (MSVC)
target_compile_options(CMP_Framework PRIVATE /Zc:strictStrings-)
endif()

if(CMP_HOST_WINDOWS)
    target_compile_definitions(CMP_Framework PUBLIC
        -DCMP_USE_XMMINTRIN
    )
endif()

file(GLOB_RECURSE HALF_SRC
    ${CMAKE_CURRENT_LIST_DIR}/common/half/*.h
    ${CMAKE_CURRENT_LIST_DIR}/common/half/*.cpp
)

file(GLOB_RECURSE DDS_SRC
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/cimage/dds/*.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/cimage/dds/*.cpp
)

file(GLOB_RECURSE HPC_SRC
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_encode/hpc/*.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_encode/hpc/*.cpp
)

file(GLOB_RECURSE CMP_FRAMEWORK_SRC
    ${CMAKE_CURRENT_LIST_DIR}/../cmp_framework/*.h
    ${CMAKE_CURRENT_LIST_DIR}/../cmp_framework/*.cpp
)

file(GLOB_RECURSE CMP_FRAMEWORK_COMMON_SRC
    ${CMAKE_CURRENT_LIST_DIR}/../cmp_framework/common/*.h
    ${CMAKE_CURRENT_LIST_DIR}/../cmp_framework/common/*.cpp
)

target_sources(CMP_Framework
    PRIVATE
    ${HALF_SRC}
    ${DDS_SRC}
    ${HPC_SRC}
    ${CMP_FRAMEWORK_SRC}
    ${CMP_FRAMEWORK_COMMON_SRC}
    
    # CMP_Math
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_libs/cmp_math/cmp_math_common.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_libs/cmp_math/cmp_math_common.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_libs/cmp_math/cpu_extensions.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_libs/cmp_math/cpu_extensions.cpp
    
    # CMP_COMMON
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/atiformats.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/atiformats.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/pluginbase.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/plugininterface.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/pluginmanager.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/pluginmanager.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/cpu_timing.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/cpu_timing.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/tc_pluginapi.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/tc_plugininternal.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/tc_plugininternal.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/utilfuncs.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/utilfuncs.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/cmp_fileio.h 
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/cmp_fileio.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/format_conversion.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/format_conversion.cpp

    # CMP_SDK Lossy Compression
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bcn.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bcn.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc1/bc1.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc1/bc1.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc2/bc2.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc2/bc2.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc3/bc3.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc3/bc3.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc4/bc4.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc4/bc4.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc5/bc5.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc5/bc5.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc6/bc6h.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc6/bc6h.cpp
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc7/bc7.h
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/bc7/bc7.cpp

    # External Sources
    ${CMAKE_CURRENT_LIST_DIR}/../external/stb/stb_image.h
    ${CMAKE_CURRENT_LIST_DIR}/../external/stb/stb_image_write.h    
)


if (OPTION_CMP_GTC)
    target_sources(CMP_Framework
        PRIVATE
        # Extra 
        ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/gtc/gtc.h
        ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/gtc/gtc.cpp
    )
endif()

if (OPTION_BUILD_BROTLIG)
    target_sources(CMP_Framework
        PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/brlg/codec_brlg.h
        ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/brlg/codec_brlg.cpp
    )

    target_link_libraries(CMP_Framework
        PRIVATE
        ExtBrotlig
    )
endif()

if (OPTION_CMP_APC)
    target_sources(CMP_Framework
        PRIVATE
        # Extra 
        ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/apc/apc.h
        ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk/apc/apc.cpp
    )
endif()

target_include_directories(CMP_Framework
    PRIVATE
    ./
    ${CMAKE_CURRENT_LIST_DIR}/../cmp_framework/common/half
    ${CMAKE_CURRENT_LIST_DIR}/../cmp_framework/common/
    ${CMAKE_CURRENT_LIST_DIR}/../cmp_compressonatorlib
    ${CMAKE_CURRENT_LIST_DIR}/../cmp_compressonatorlib/buffer
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/ccmp_sdk
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common
    #${CMAKE_CURRENT_LIST_DIR}/../applications/_plugins/common/gltf
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_libs/cmp_math
    ${CMAKE_CURRENT_LIST_DIR}/../applications/_libs/gpu_decode
    ${CMAKE_CURRENT_LIST_DIR}/../external/stb
)

if (UNIX)
    target_compile_definitions(CMP_Framework PRIVATE _LINUX)
endif()

set_target_properties(CMP_Framework PROPERTIES FOLDER "Libs")

target_link_libraries(CMP_Framework PRIVATE CMP_Core)