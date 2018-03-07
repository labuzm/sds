cmake_minimum_required(VERSION 3.0)

project(sds)

set(SRC_H_FILES sds.h sdsalloc.h)
set(SRC_FILES ${SRC_H_FILES} sds.c)
add_library(sds-shared SHARED ${SRC_FILES})
set_target_properties(sds-shared PROPERTIES OUTPUT_NAME sds)
add_library(sds-static STATIC ${SRC_FILES})
set_target_properties(sds-static PROPERTIES OUTPUT_NAME sds)

target_include_directories(sds-shared PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

target_include_directories(sds-static PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

install(TARGETS sds-shared sds-static
        EXPORT sds-config
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)
install(FILES ${SRC_H_FILES} DESTINATION include/sds)

install(EXPORT sds-config DESTINATION lib/cmake/sds)
export(TARGETS sds-shared sds-static FILE sds-config.cmake)

