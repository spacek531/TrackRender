#spacek: cmake .. -G Ninja -DCMAKE_TOOLCHAIN_FILE=D:\packages\vcpkg-master\scripts\buildsystems\vcpkg.cmake -DVCPKG_TARGET_TRIPLET=x64-windows -DCMAKE_BUILD_TYPE=Release; Ninja

cmake_minimum_required (VERSION 3.1)

project (TrackRender C)

find_package(libpng REQUIRED)
find_package(jansson REQUIRED)
find_package(embree 3 REQUIRED)
find_package(assimp REQUIRED)

if (MSVC) # assumes vcpkg
    set(EMBREE ${EMBREE_LIBRARIES})
    set(ASSIMP assimp::assimp)
    set(MATH_LIB "")
    set(JANSSON jansson::jansson)
else()
    set(EMBREE embree3)
    set(ASSIMP assimp)
    set(MATH_LIB m)
    set(JANSSON jansson)
endif()
if (NOT DEFINED ISO_RENDER_DIR)
    set(ISO_RENDER_DIR D:/documents/github/libIsoRender-github-actions)
endif()
if(NOT DEFINED IMAGE)
    set(IMAGE ${ISO_RENDER_DIR}/build/libImage/image.lib)
endif()
if(NOT DEFINED ISO_RENDER)
    set(ISO_RENDER ${ISO_RENDER_DIR}/build/isoRender.lib)
endif()

include_directories(${PNG_INCLUDE_DIR} ../libIsoRender/src ../libImage/src)
link_directories(${ISO_RENDER_DIR}/build ${ISO_RENDER_DIR}/build/libImage)

add_executable(TrackRender src/main.c src/track.c src/track_sections.c src/sprites.c)
set_property(TARGET TrackRender PROPERTY C_STANDARD 99)
target_link_libraries (TrackRender ${PNG_LIBRARY} ${IMAGE} ${ISO_RENDER} ${EMBREE} ${ASSIMP} ${JANSSON} ${MATH_LIB})
