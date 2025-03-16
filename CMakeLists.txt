cmake_minimum_required(VERSION 3.15)

project(HawkNL 
    VERSION 1.7.0
    DESCRIPTION "HawkNL cross platform network library"
    HOMEPAGE_URL "https://github.com/yourusername/hawknl"
    LANGUAGES C
)

# Options
option(BUILD_SHARED_LIBS "Build shared libraries" ON)
option(HAWKNL_INCLUDE_IPX "Include IPX support (Windows only)" ON)
option(HAWKNL_INCLUDE_LOOPBACK "Include loopback support" ON)
option(HAWKNL_INCLUDE_SERIAL "Include serial support" ON)

# Set version variables
set(HAWKNL_MAJOR_VERSION 1)
set(HAWKNL_MINOR_VERSION 7)
set(HAWKNL_PATCH_VERSION 0)

# Configure platform-specific settings
if(WIN32)
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
    if(NOT HAWKNL_INCLUDE_IPX)
        add_definitions(-DNL_NO_IPX)
    endif()
else()
    add_definitions(-D_GNU_SOURCE -D_REENTRANT)
endif()

# Configure feature settings
if(NOT HAWKNL_INCLUDE_LOOPBACK)
    add_definitions(-DNL_NO_LOOPBACK)
endif()

if(NOT HAWKNL_INCLUDE_SERIAL)
    add_definitions(-DNL_NO_SERIAL)
endif()

# Define source files
set(HAWKNL_SOURCES
    src/crc.c
    src/err.c
    src/errorstr.c
    src/group.c
    src/hthread.c
    src/htcondition.c
    src/htmutex.c
    src/loopback.c
    src/nl.c
    src/nltime.c
    src/sock.c
)

# Add Windows-specific source files
if(WIN32)
    list(APPEND HAWKNL_SOURCES src/ipx.c)
endif()

# Define the library
add_library(NL ${HAWKNL_SOURCES})

# Set include directories
target_include_directories(NL
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

# Set library properties
set_target_properties(NL PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${HAWKNL_MAJOR_VERSION}
    C_STANDARD 99
    POSITION_INDEPENDENT_CODE ON
    EXPORT_NAME HawkNL
)

# Link with platform-specific libraries
if(WIN32)
    target_link_libraries(NL PRIVATE wsock32 winmm)
else()
    target_link_libraries(NL PRIVATE pthread)
endif()

# Install targets
include(GNUInstallDirs)

install(TARGETS NL
    EXPORT HawkNLTargets
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(FILES 
    include/nl.h
    include/hawklib.h
    include/hawkthreads.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

# Export targets
install(EXPORT HawkNLTargets
    FILE HawkNLTargets.cmake
    NAMESPACE HawkNL::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/HawkNL
)

# Create and install config file
include(CMakePackageConfigHelpers)

configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/HawkNLConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/HawkNLConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/HawkNL
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/HawkNLConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/HawkNLConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/HawkNLConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/HawkNL
) 