cmake_minimum_required (VERSION 3.5)

# Set the project name
project (add_module)

# Add a library with the above sources
add_library(${PROJECT_NAME} src/add.cpp)

target_include_directories( ${PROJECT_NAME}
    PUBLIC ${PROJECT_SOURCE_DIR}/include
)

# Installation (https://github.com/forexample/package-example) {

    set(config_install_dir "lib/cmake/${PROJECT_NAME}")
    set(include_install_dir "include")
    
    set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")
    
    set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
    set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
    set(TARGETS_EXPORT_NAME "${PROJECT_NAME}Targets")
    set(namespace "${PROJECT_NAME}::")
    
    include(CMakePackageConfigHelpers)
    
    # Use:
    #   * PROJECT_VERSION
    write_basic_package_version_file(
        "${version_config}" COMPATIBILITY SameMajorVersion
    )
    
    # Use:
    #   * TARGETS_EXPORT_NAME
    #   * PROJECT_NAME
    configure_package_config_file(
        "cmake/Config.cmake.in"
        "${project_config}"
        INSTALL_DESTINATION "${config_install_dir}"
    )
    
    install(
        TARGETS dummy-cmake-project
        EXPORT "${TARGETS_EXPORT_NAME}"
        LIBRARY DESTINATION "lib"
        ARCHIVE DESTINATION "lib"
        RUNTIME DESTINATION "bin"
        INCLUDES DESTINATION "${include_install_dir}"
    )
    
    install(
        FILES include/add.h
        DESTINATION "${include_install_dir}"
    )
    
    install(
        FILES "${project_config}" "${version_config}"
        DESTINATION "${config_install_dir}"
    )
    
    install(
        EXPORT "${TARGETS_EXPORT_NAME}"
        NAMESPACE "${namespace}"
        DESTINATION "${config_install_dir}"
    )
    
    # }