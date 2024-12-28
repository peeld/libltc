cmake_minimum_required(VERSION 3.14)
include(GenerateExportHeader)

project(LibLtc)

set(CMAKE_DEBUG_POSTFIX d)

include_directories (${PROJECT_BINARY_DIR})
file (GLOB SOURCES "src/*.c")
file (GLOB HEADERS "src/*.h")

# LGPL - Building as shared.
add_library(ltc SHARED ${SOURCES} ${HEADERS})

target_compile_definitions(ltc PRIVATE LTC_EXPORTS)

generate_export_header(ltc)

# Specify installation paths
install(TARGETS ltc
    EXPORT ltcTargets
    RUNTIME DESTINATION bin          # For .dll
    LIBRARY DESTINATION lib          # For .so or .dll (on some configurations)
    ARCHIVE DESTINATION lib          # For .lib	
	COMPONENT library
)

# Install header files
install(FILES ${HEADERS} ${PROJECT_BINARY_DIR}/ltclib_export.h
    DESTINATION include
)

# Export the library for use with other projects
install(EXPORT ltcTargets
    FILE ltcConfig.cmake
    DESTINATION lib/cmake/ltc
)