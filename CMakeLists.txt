cmake_minimum_required(VERSION 3.0)

project(MadgwickAHRS VERSION 1.2.0)
#set(PROJECT_VERSION_MAJOR 1)
#set(PROJECT_VERSION_MINOR 2)
#set(PROJECT_VERSION_PATCH 0)

add_library(${PROJECT_NAME} src/MadgwickAHRS.cpp)
target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
    $<INSTALL_INTERFACE:include>
)

install(FILES src/MadgwickAHRS.h DESTINATION include)
install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}Targets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

# Install config-related files
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ${PROJECT_NAME}Version.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)
configure_file(cmake/Config.cmake.in ${PROJECT_NAME}Config.cmake @ONLY)
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Version.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    DESTINATION share/cmake/${PROJECT_NAME}
)
install(EXPORT ${PROJECT_NAME}Targets
    DESTINATION share/cmake/${PROJECT_NAME}
    NAMESPACE MadgwickAHRS::
)
