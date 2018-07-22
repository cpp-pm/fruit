
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

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
  "${VERSION_CONFIG}" COMPATIBILITY SameMajorVersion
)

configure_package_config_file("${PROJECT_NAME}Config.cmake.in"
  ${PROJECT_CONFIG}
  INSTALL_DESTINATION ${INSTALL_CONFIG_DIR}
  PATH_VARS INSTALL_CONFIG_DIR
)

install(
  DIRECTORY "${CMAKE_SOURCE_DIR}/include/"
  DESTINATION "${INSTALL_INCLUDE_DIR}"
)

install(
  FILES "${PROJECT_CONFIG}" "${VERSION_CONFIG}"
  DESTINATION "${INSTALL_CONFIG_DIR}"
)

install(
  EXPORT "${TARGETS_EXPORT_NAME}"
  NAMESPACE "${PROJECT_NAME}::"
  DESTINATION "${INSTALL_CONFIG_DIR}"
)

install(
  TARGETS fruit
  EXPORT "${TARGETS_EXPORT_NAME}"
  ARCHIVE DESTINATION "${INSTALL_LIBRARY_DIR}"
  LIBRARY DESTINATION "${INSTALL_LIBRARY_DIR}"
  INCLUDES DESTINATION "${INSTALL_INCLUDE_DIR}"
)
