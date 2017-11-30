cmake_minimum_required(VERSION 3.0)
#set(CMAKE_VERBOSE_MAKEFILE ON)

if (NOT CMAKE_SYSTEM_NAME)
    set(CMAKE_SYSTEM_NAME "Generic")
endif (NOT CMAKE_SYSTEM_NAME)

# delete cmake cache folder before changing this options
option(BUILD_SFML "Build with SFML support" OFF)
option(BUILD_SDL2 "Build with SDL2 support" OFF)
option(BUILD_PSP2 "Build with PSP2 support" OFF)
option(BUILD_PS3 "Build with PS3 support" OFF)
option(BUILD_3DS "Build with 3DS support" ON)
option(BUILD_RPI "Build with RPI support" OFF)
option(BUILD_NX "Build with NX (SWITCH) support" OFF)

if (BUILD_PSP2)
    set(VITASDK $ENV{VITASDK})
    set(CMAKE_SYSTEM_PROCESSOR "armv7-a")
    set(CMAKE_C_COMPILER "${VITASDK}/bin/arm-vita-eabi-gcc")
    set(CMAKE_CXX_COMPILER "${VITASDK}/bin/arm-vita-eabi-g++")
    set(CMAKE_ASM_COMPILER "${VITASDK}/bin/arm-vita-eabi-g++")
    set(CMAKE_AR "${VITASDK}/bin/arm-vita-eabi-ar")
    set(CMAKE_RANLIB "${VITASDK}/bin/arm-vita-eabi-ranlib")
    set(CMAKE_C_FLAGS "-Wl,-q" CACHE STRING "C flags")
    execute_process(COMMAND "date" "+%Y.%m.%d" OUTPUT_VARIABLE BUILD_DATE OUTPUT_STRIP_TRAILING_WHITESPACE)
    set(TITLE_ID "CROSS2D00")
elseif (BUILD_PS3)
    set(PS3DEV /usr/local/ps3dev)
    set(PSL1GHT ${PS3DEV})
    set(ENV{PSL1GHT} ${PSL1GHT})
    set(CMAKE_SYSTEM_PROCESSOR "powerpc64")
    set(CMAKE_C_COMPILER "${PSL1GHT}/ppu/bin/powerpc64-ps3-elf-gcc")
    set(CMAKE_CXX_COMPILER "${PSL1GHT}/ppu/bin/powerpc64-ps3-elf-g++")
    set(CMAKE_ASM_COMPILER "${PSL1GHT}/ppu/bin/powerpc64-ps3-elf-gcc")
    set(CMAKE_AR "${PSL1GHT}/ppu/bin/powerpc64-ps3-elf-ar")
    set(CMAKE_RANLIB "${PSL1GHT}/ppu/bin/powerpc64-ps3-elf-ranlib")
    set(CMAKE_C_FLAGS "-mcpu=cell" CACHE STRING "C flags")
    set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -std=gnu++11 -fpermissive" CACHE STRING "C++ flags")
    set(CMAKE_FIND_ROOT_PATH ${PSL1GHT}/ppu ${PSL1GHT}/portlibs/ppu)
    set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM ONLY)
    set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
    set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
elseif (BUILD_3DS)
    set(DEVKITPRO $ENV{DEVKITPRO})
    set(CMAKE_SYSTEM_PROCESSOR "armv6k")
    set(CMAKE_C_COMPILER "${DEVKITPRO}/devkitARM/bin/arm-none-eabi-gcc")
    set(CMAKE_CXX_COMPILER "${DEVKITPRO}/devkitARM/bin/arm-none-eabi-g++")
    set(CMAKE_ASM_COMPILER "${DEVKITPRO}/devkitARM/bin/arm-none-eabi-as")
    set(CMAKE_AR "${DEVKITPRO}/devkitARM/bin/arm-none-eabi-gcc-ar" CACHE STRING "")
    set(CMAKE_RANLIB "${DEVKITPRO}/devkitARM/bin/arm-none-eabi-gcc-ranlib" CACHE STRING "")
    set(CMAKE_C_FLAGS "-march=armv6k -mtune=mpcore -mfloat-abi=hard -mtp=soft -mword-relocations" CACHE STRING "C flags")
    set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -fpermissive -std=gnu++11 -fno-rtti -fno-exceptions" CACHE STRING "C++ flags")
    set(CMAKE_FIND_ROOT_PATH ${DEVKITPRO} ${DEVKITPRO}/devkitARM ${DEVKITPRO}/libctru ${DEVKITPRO}/portlibs/3ds ${DEVKITPRO}/portlibs/armv6k)
    set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
    set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
    set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
    set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)
    set(BUILD_SHARED_LIBS OFF CACHE INTERNAL "Shared libs not available")
elseif (BUILD_NX)
    set(DEVKITPRO $ENV{DEVKITPRO})
    set(CMAKE_SYSTEM_PROCESSOR "armv8-a")
    set(CMAKE_C_COMPILER "${DEVKITPRO}/devkitA64/bin/aarch64-none-elf-gcc")
    set(CMAKE_CXX_COMPILER "${DEVKITPRO}/devkitA64/bin/aarch64-none-elf-g++")
    set(CMAKE_ASM_COMPILER "${DEVKITPRO}/devkitA64/bin/aarch64-none-elf-as")
    set(CMAKE_AR "${DEVKITPRO}/devkitA64/bin/aarch64-none-elf-gcc-ar" CACHE STRING "")
    set(CMAKE_RANLIB "${DEVKITPRO}/devkitA64/bin/aarch64-none-elf-gcc-ranlib" CACHE STRING "")
    set(CMAKE_C_FLAGS "-march=armv8-a -mtp=soft -fPIE -ffast-math -ffunction-sections -fdata-sections" CACHE STRING "C flags")
    set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -fpermissive -fno-rtti -fno-exceptions -std=gnu++11" CACHE STRING "C++ flags")
    set(CMAKE_FIND_ROOT_PATH ${DEVKITPRO} ${DEVKITPRO}/devkitA64 ${DEVKITPRO}/portlibs/armv8-a)
    set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
    set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
    set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
    set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)
    set(BUILD_SHARED_LIBS OFF CACHE INTERNAL "Shared libs not available")
elseif (BUILD_RPI)
    set(RPISDK "/usr/local/rpi-tools")
    set(CMAKE_SYSTEM_PROCESSOR "armv7-a")
    set(CMAKE_C_COMPILER "${RPISDK}/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian/bin/arm-linux-gnueabihf-gcc")
    set(CMAKE_CXX_COMPILER "${RPISDK}/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian/bin/arm-linux-gnueabihf-g++")
    set(CMAKE_ASM_COMPILER "${RPISDK}/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian/bin/arm-linux-gnueabihf-g++")
    set(CMAKE_C_FLAGS "-I${RPISDK}/sysroot/include" CACHE STRING "C flags")
endif (BUILD_PSP2)

project(cross2d)

# flags
set(FLAGS)

##############
# CROSS2D
##############
set(CROSS2D_DIR src src/skeleton)
file(GLOB CROSS2D_SRC src/skeleton/*.c*)

#####################
# PLATORM SPECIFIC
#####################
#####################
# PSP2 PLATORM
#####################
if (BUILD_PSP2)
    set(PLATFORM_DIRS
            src/psp2
            src/psp2/libvita2d/include
            src/psp2/vita-shader-collection/includes
            ${VITASDK}/arm-vita-eabi/include/freetype2
            )
    file(GLOB PLATFORM_SRC
            src/psp2/*.c*
            src/psp2/libvita2d/source/*.c
            src/sdl2/sdl2_audio.cpp # TODO: get rid of sdl2, use sdl2 audio for now
            src/sdl2/sdl2_input.cpp # TODO: get rid of sdl2, use sdl2 input for now
            src/psp2/libvita2d/shader/compiled/*.o
            src/psp2/vita-shader-collection/lib/*.o
            )
    list(APPEND FLAGS -Wl,-q -O3 -D__PSP2__)
    if (BUILD_DEBUG)
        list(APPEND FLAGS -D__PSP2_DEBUG__)
    endif (BUILD_DEBUG)
    #####################
    # PS3 PLATORM
    #####################
elseif (BUILD_PS3)
    set(PLATFORM_DIRS
            src/sdl2
            src/posix
            ${PSL1GHT}/ppu/include
            ${PSL1GHT}/portlibs/ppu/include
            )
    file(GLOB PLATFORM_SRC
            src/sdl2/*.c*
            src/posix/*.c*
            )
    list(APPEND FLAGS -O3 -D__SDL2__)
    #####################
    # 3DS PLATORM
    #####################
elseif (BUILD_3DS)
    # shader
    #include(src/3ds/cmake/Tools3DS.cmake)
    #set(SHADER_AS picasso)
    #add_shbin_library(shaders.3ds src/3ds/vshader.v.pica)
    set(PLATFORM_DIRS
            src/3ds
            src/posix
            ${DEVKITPRO}/libctru/include
            ${DEVKITPRO}/portlibs/3ds/include
            ${DEVKITPRO}/portlibs/armv6k/include
            )
    file(GLOB PLATFORM_SRC
            src/3ds/*.c*
            src/posix/*.c*
            src/sdl2/sdl2_audio.cpp # TODO: get rid of sdl2, use sdl2 audio for now
            # TODO: fix that crap...
            src/3ds/vshaders.v.o
            )
    list(APPEND FLAGS -O3 -DARM11 -D_3DS -D__3DS__
            -ffunction-sections -fomit-frame-pointer
            )
    #####################
    # NX PLATORM
    #####################
elseif (BUILD_NX)
    set(PLATFORM_DIRS
            src/nx
            ${DEVKITPRO}/libnx/include
            )
    file(GLOB PLATFORM_SRC
            src/nx/*.c*
            )
    list(APPEND FLAGS -D_NX -D__NX__
            -ffunction-sections -fomit-frame-pointer
            )
    #####################
    # SDL2 PLATORM
    #####################
elseif (BUILD_SDL2)
    set(PLATFORM_DIRS
            src/sdl2
            src/posix
            )
    file(GLOB PLATFORM_SRC
            src/sdl2/*.c*
            src/posix/*.c*
            )
    list(APPEND FLAGS -O3 -D__SDL2__)
    #####################
    # SFML PLATORM
    #####################
elseif (BUILD_SFML)
    set(PLATFORM_DIRS
            src/sfml
            src/posix
            )
    file(GLOB PLATFORM_SRC
            src/sfml/*.c*
            src/posix/*.c*
            src/sdl2/sdl2_audio.cpp #TODO: get rid of sdl2, use sdl2 audio for now
            )
    list(APPEND FLAGS -O3 -D__SFML__)
endif (BUILD_PSP2)


add_library(${PROJECT_NAME} ${CROSS2D_SRC} ${PLATFORM_SRC})
target_include_directories(${PROJECT_NAME} PUBLIC ${CROSS2D_DIR})
target_include_directories(${PROJECT_NAME} PRIVATE ${PLATFORM_DIRS})
target_compile_options(${PROJECT_NAME} PRIVATE ${FLAGS})

#####################
# SDL2 (unix) target
#####################
add_executable(${PROJECT_NAME}.sdl2 src/main.cpp)
target_compile_options(${PROJECT_NAME}.sdl2 PRIVATE ${FLAGS})
target_link_libraries(${PROJECT_NAME}.sdl2
        cross2d SDL2 SDL2_image SDL2_ttf z)
add_dependencies(${PROJECT_NAME}.sdl2 ${PROJECT_NAME})

#####################
# PS3 target
#####################
add_executable(${PROJECT_NAME}.ps3 src/main.cpp)
target_compile_options(${PROJECT_NAME}.ps3 PRIVATE ${FLAGS})
target_link_libraries(${PROJECT_NAME}.ps3
        cross2d SDL2 SDL2_image SDL2_ttf z)
add_dependencies(${PROJECT_NAME}.ps3 ${PROJECT_NAME})

#####################
# SFML (unix) target
#####################
add_executable(${PROJECT_NAME}.sfml src/main.cpp)
target_compile_options(${PROJECT_NAME}.sfml PRIVATE ${FLAGS})
target_link_libraries(${PROJECT_NAME}.sfml
        cross2d SDL2 sfml-graphics sfml-window sfml-system GL png z)
add_dependencies(${PROJECT_NAME}.sfml ${PROJECT_NAME})

#####################
# PSP2 (vita) target
#####################
add_executable(${PROJECT_NAME}.psp2 src/main.cpp)
target_compile_options(${PROJECT_NAME}.psp2 PRIVATE ${FLAGS})
target_link_libraries(${PROJECT_NAME}.psp2
        cross2d SDL2
        SceDisplay_stub SceGxm_stub SceSysmodule_stub
        SceCtrl_stub ScePgf_stub SceNetCtl_stub
        SceNet_stub ScePower_stub SceCommonDialog_stub
        SceAudio_stub SceAppMgr_stub SceTouch_stub
        stdc++ png jpeg z m c
        )
add_dependencies(${PROJECT_NAME}.psp2 ${PROJECT_NAME})
add_custom_target(${PROJECT_NAME}.psp2.vpk
        DEPENDS ${PROJECT_NAME}.psp2
        COMMAND rm -rf vpk && mkdir -p vpk/sce_sys
        COMMAND ${VITASDK}/bin/vita-elf-create ${PROJECT_NAME}.psp2 ${PROJECT_NAME}.velf
        COMMAND ${VITASDK}/bin/vita-make-fself -c ${PROJECT_NAME}.velf vpk/eboot.bin
        COMMAND ${VITASDK}/bin/vita-mksfoex -s TITLE_ID=${TITLE_ID} "${PROJECT_NAME}" vpk/sce_sys/param.sfo
        COMMAND cp -r ${CMAKE_CURRENT_SOURCE_DIR}/src/res/* vpk/
        COMMAND cd vpk && zip -r ../${PROJECT_NAME}-${BUILD_DATE}.vpk . && cd ..
        )

#####################
# CTR (3DS) target
#####################
add_executable(${PROJECT_NAME}.3ds src/main.cpp)
target_compile_options(${PROJECT_NAME}.3ds PRIVATE ${FLAGS})
target_include_directories(${PROJECT_NAME}.3ds PUBLIC ${CROSS2D_DIR})
target_include_directories(${PROJECT_NAME}.3ds PRIVATE ${PLATFORM_DIRS})
target_link_libraries(${PROJECT_NAME}.3ds
        cross2d
        ${DEVKITPRO}/portlibs/3ds/lib/libSDL2.a
        ${DEVKITPRO}/portlibs/armv6k/lib/libpng.a
        ${DEVKITPRO}/portlibs/armv6k/lib/libz.a
        ${DEVKITPRO}/libctru/lib/libcitro3d.a
        ${DEVKITPRO}/libctru/lib/libctru.a
        m)
set_target_properties(${PROJECT_NAME}.3ds PROPERTIES LINK_FLAGS "-specs=3dsx.specs")
add_dependencies(${PROJECT_NAME}.3ds ${PROJECT_NAME})
include(src/3ds/cmake/Tools3DS.cmake)
add_3dsx_target(${PROJECT_NAME}.3ds NO_SMDH)

#####################
# NX (SWITCH) target
#####################
add_executable(${PROJECT_NAME}.nx src/main.cpp)
target_compile_options(${PROJECT_NAME}.nx PRIVATE ${FLAGS} -I${DEVKITPRO}/libnx/include)
target_link_libraries(${PROJECT_NAME}.nx
        cross2d
        ${DEVKITPRO}/libnx/lib/libnx.a
        m)
set_target_properties(${PROJECT_NAME}.nx PROPERTIES LINK_FLAGS "-specs=switch.specs")
add_dependencies(${PROJECT_NAME}.nx ${PROJECT_NAME})