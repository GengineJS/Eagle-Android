cmake_minimum_required(VERSION 3.4.1 FATAL_ERROR)
set(PROJECT_NAME EagleEngine)
set(ENGINE_VISION 0.1)
add_definitions(-DENGINE_NAME="${PROJECT_NAME}")
add_definitions(-DENGINE_VERSION=${ENGINE_VISION})

set(NAME blur)

set(SRC_DIR ../../../examples/${NAME})
# engine source
set(BASE_DIR ../../../core)
set(GRAPHICS_DIR ${BASE_DIR}/graphics)
set(UTILS_DIR ${BASE_DIR}/utils)
set(PIPELINE_DIR ${BASE_DIR}/pipeline)

set(EXTERNAL_DIR ../../../external)
# select gfx api
# Use vulkan api by default
set(USE_GFX_VULKAN ON)
set(USE_GFX_GLES2 OFF)
set(USE_GFX_GLES3 OFF)
set(USE_GFX_WEBGL OFF)
set(USE_GFX_WEBGPU OFF)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -DUSE_PLATFORM_ANDROID_KHR -DVK_NO_PROTOTYPES")
if(USE_GFX_VULKAN)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DUSE_GFX_VULKAN")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DVK_USE_PLATFORM_ANDROID_KHR")
endif()
file(GLOB EXAMPLE_SRC "${SRC_DIR}/*.cpp")

add_library(native-app-glue STATIC ${ANDROID_NDK}/sources/android/native_app_glue/android_native_app_glue.c)

add_subdirectory(../base ${CMAKE_SOURCE_DIR}/../base)
add_library(eagle-sample SHARED ${EXAMPLE_SRC})
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -u ANativeActivity_onCreate")

# engine source
include_directories(${BASE_DIR})
include_directories(${GRAPHICS_DIR})
include_directories(${UTILS_DIR})
include_directories(${PIPELINE_DIR})

include_directories(${EXTERNAL_DIR})
include_directories(${EXTERNAL_DIR}/glm)
include_directories(${EXTERNAL_DIR}/imgui)
include_directories(${ANDROID_NDK}/sources/android/native_app_glue)

target_link_libraries(
    eagle-sample
    native-app-glue
    eagle
    android
    log
    z
)
