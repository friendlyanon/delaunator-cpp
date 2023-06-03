cmake_minimum_required(VERSION 3.14)

project(delaunator VERSION 0.4.0 LANGUAGES NONE)

add_library(delaunator INTERFACE)
target_include_directories(delaunator INTERFACE "\$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>")

target_compile_features(delaunator INTERFACE cpp_std_17)

if(NOT CMAKE_SKIP_INSTALL_RULES)
  set(CMAKE_INSTALL_DEFAULT_COMPONENT_NAME delaunator_Development)

  # Avoid using a common include directory
  set(CMAKE_INSTALL_INCLUDEDIR "include/delaunator-${PROJECT_VERSION}" CACHE PATH "")

  # Needed by GNUInstallDirs, because LANGUAGES is NONE
  set(CMAKE_INSTALL_LIBDIR "" CACHE PATH "")

  include(CMakePackageConfigHelpers)
  include(GNUInstallDirs)

  install(DIRECTORY include/ DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")
  install(TARGETS delaunator EXPORT delaunatorTargets INCLUDES DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")

  write_basic_package_version_file(delaunatorConfigVersion.cmake COMPATIBILITY SameMajorVersion ARCH_INDEPENDENT)
  set(delaunator_INSTALL_CMAKEDIR "${CMAKE_INSTALL_DATADIR}/delaunator" CACHE PATH "")
  mark_as_advanced(delaunator_INSTALL_CMAKEDIR)

  install(FILES "${PROJECT_BINARY_DIR}/delaunatorConfigVersion.cmake" DESTINATION "${delaunator_INSTALL_CMAKEDIR}")
  install(EXPORT delaunatorTargets NAMESPACE delaunator:: DESTINATION "${delaunator_INSTALL_CMAKEDIR}" FILE delaunatorConfig.cmake)

  include(CPack)
endif()
