
set(FRUIT_SOURCES
        memory_pool.cpp
binding_normalization.cpp
demangle_type_name.cpp
component.cpp
fixed_size_allocator.cpp
injector_storage.cpp
normalized_component_storage.cpp
normalized_component_storage_holder.cpp
semistatic_map.cpp
semistatic_graph.cpp)

if("${BUILD_SHARED_LIBS}")
    add_library(fruit SHARED ${FRUIT_SOURCES})

    if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
        set_target_properties(fruit PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
    endif()
else()
    add_library(fruit STATIC ${FRUIT_SOURCES})
endif()

####
# Installation (https://github.com/forexample/package-example)

set(config_install_dir "lib/cmake/${PROJECT_NAME}")

set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(targets_export_name "${PROJECT_NAME}Targets")
set(namespace "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)

configure_package_config_file("${PROJECT_NAME}Config.cmake.in"
  ${project_config}
  INSTALL_DESTINATION ${config_install_dir}
  PATH_VARS INSTALL_INCLUDE_DIR)

# Note: PROJECT_VERSION is used as a VERSION
write_basic_package_version_file(
    "${version_config}" COMPATIBILITY SameMajorVersion
)

install(TARGETS fruit
        EXPORT "${targets_export_name}"
        ARCHIVE DESTINATION "${INSTALL_LIBRARY_DIR}"
        LIBRARY DESTINATION "${INSTALL_LIBRARY_DIR}")

install(
    FILES "${project_config}" "${version_config}"
    DESTINATION "${config_install_dir}"
)

install(
    EXPORT "${targets_export_name}"
    NAMESPACE "${namespace}"
    DESTINATION "${config_install_dir}"
)
