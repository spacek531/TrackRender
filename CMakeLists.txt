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
else()
    set(EMBREE embree3)
    set(ASSIMP assimp)
    set(MATH_LIB m)
endif()

set(IMAGE ../libIsoRender/build/libImage/image)
set(ISO_RENDER ../libIsoRender/build/isoRender)

include_directories(${PNG_INCLUDE_DIR} ../libIsoRender/src ../libImage/src)
link_directories(../libIsoRender/build ../libImage)

add_executable(TrackRender src/main.c src/track.c src/track_sections.c src/sprites.c)
set_property(TARGET TrackRender PROPERTY C_STANDARD 99)
target_link_libraries (TrackRender ${IMAGE} ${ISO_RENDER} ${PNG_LIBRARY} ${EMBREE} ${ASSIMP} ${JANSSON} ${MATH_LIB})
