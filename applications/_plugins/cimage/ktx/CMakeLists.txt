cmake_minimum_required(VERSION 3.10)

add_library(Image_KTX STATIC "")

# Enabled KTX1 Only
file(GLOB_RECURSE KTX_Lib
    lib/*.h
    lib/*.cpp
    lib/*.c 
    )

target_sources(Image_KTX
    PRIVATE
    ${KTX_Lib}
    ./ktx1.cpp
    ./ktx1.h
    ./softfloat.cpp
    ./softfloat.h
    )

target_include_directories(Image_KTX
    PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/../../../../cmp_compressonatorlib
    ${CMAKE_CURRENT_LIST_DIR}/../../../../applications/_libs/gpu_decode
    ${CMAKE_CURRENT_LIST_DIR}/../../../../cmp_framework/common/half
    ${CMAKE_CURRENT_LIST_DIR}/../../../../applications/_plugins/common

    ${CMAKE_CURRENT_LIST_DIR}/../../../../common/lib/ext/glew/1.9.0/include
    ./lib/
    ./
    )


# Enabled KTX2 Features
if (CMP_HOST_WINDOWS)
    target_include_directories(Image_KTX
      PRIVATE
      ${CMAKE_CURRENT_LIST_DIR}/../../../../common/lib/ext/ktx/include
      ${CMAKE_CURRENT_LIST_DIR}/../../../../common/lib/ext/ktx/lib
      ${VULKAN_SDK_PATH}/include/
      )

    target_link_libraries(Image_KTX PRIVATE
        CMP_Compressonator
        CMP_Framework
        ExtKTX
        )
endif()

if (UNIX)
    target_compile_definitions(Image_KTX PRIVATE _LINUX)
    find_package(OpenGL) 
    if (OpenGL_FOUND)
        if(APPLE)
            target_include_directories(Image_KTX
                                       PRIVATE
                                       /usr/local/include)
        endif()
    endif()
endif()

set_target_properties(Image_KTX PROPERTIES 
    FOLDER "Plugin_Static/ImageIO"
    # USE_FOLDERS ON
    )

target_link_directories(Image_KTX PUBLIC ${CMAKE_CURRENT_LIST_DIR}/../../../../common/lib/ext/glew/1.9.0/lib/x64/)