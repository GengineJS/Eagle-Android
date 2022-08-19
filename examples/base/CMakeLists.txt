file(GLOB BASE_SRC "${BASE_DIR}/*.cpp" "../../../external/imgui/*.cpp")
file(GLOB BASE_HEADERS "${BASE_DIR}/*.hpp" "${BASE_DIR}/*.h")
file(GLOB GRAPHICS "${GRAPHICS_DIR}/*.cpp"
        "${GRAPHICS_DIR}/*.hpp"
        "${GRAPHICS_DIR}/*.h"
        "${GRAPHICS_DIR}/vulkan/*.cpp"
        "${GRAPHICS_DIR}/vulkan/*.hpp"
        "${GRAPHICS_DIR}/vulkan/*.h")
file(GLOB UTILS "${UTILS_DIR}/*.cpp"
        "${UTILS_DIR}/*.hpp"
        "${UTILS_DIR}/*.h")
file(GLOB PIPELINE "${PIPELINE_DIR}/*.cpp"
        "${PIPELINE_DIR}/*.hpp"
        "${PIPELINE_DIR}/*.h"
        "${PIPELINE_DIR}/render-graph/*.cpp"
        "${PIPELINE_DIR}/render-graph/*.hpp"
        "${PIPELINE_DIR}/render-graph/*.h")
# Whether to enable verification
set(validation false)
add_definitions(-DVALIDATION=${validation})

add_library(eagle SHARED ${BASE_SRC} ${GRAPHICS} ${UTILS} ${PIPELINE})

include_directories(${BASE_DIR})
include_directories(${GRAPHICS_DIR})
include_directories(${UTILS_DIR})
include_directories(${PIPELINE_DIR})

include_directories(../../../external)
include_directories(../../../external/glm)
include_directories(../../../external/gli)
include_directories(../../../external/imgui)
include_directories(${EXTERNAL_DIR}/tinygltf)
include_directories(${ANDROID_NDK}/sources/android/native_app_glue)

set(KTX_DIR ../../../external/ktx)
set(KTX_SOURCES
	${KTX_DIR}/lib/texture.c
	${KTX_DIR}/lib/hashlist.c
	${KTX_DIR}/lib/checkheader.c
	${KTX_DIR}/lib/swap.c
	${KTX_DIR}/lib/memstream.c
	${KTX_DIR}/lib/filestream.c
)
set(KTX_INCLUDE
	${KTX_DIR}/include
	${KTX_DIR}/lib
	${KTX_DIR}/other_include
)

add_library(libktx ${KTX_SOURCES})
target_include_directories(libktx PUBLIC ${KTX_INCLUDE})
set_property(TARGET libktx PROPERTY FOLDER "external")


target_link_libraries(
	eagle
	android
	log
	z
	libktx
)
