# ----------------------------------------------------------------------------
#  CMake file for python support
# ----------------------------------------------------------------------------
if (CMAKE_CXX_COMPILER_ID STREQUAL GNU)
  # Create a version script for the GNU ld.
  set(CMAKE_VERBOSE_MAKEFILE ON)
  set(_symbols "OPENCV3.3.0 { global: extern \"C++\" {*;}; *;  };")
  set(_version_script "${CMAKE_CURRENT_BINARY_DIR}/version.script")
  file(WRITE ${_version_script} "${_symbols}\n")

  set(CMAKE_SHARED_LINKER_FLAGS " -Wl,--version-script,\"${_version_script}\"")
endif()
if(ANDROID OR APPLE_FRAMEWORK OR WINRT)
  set(__disable_python2 ON)
  set(__disable_python3 ON)
elseif(BUILD_opencv_world OR (WIN32 AND CMAKE_BUILD_TYPE STREQUAL "Debug"))
  if(NOT DEFINED BUILD_opencv_python2)
    set(__disable_python2 ON)
  endif()
  if(NOT DEFINED BUILD_opencv_python3)
    set(__disable_python3 ON)
  endif()
endif()

if(__disable_python2)
  ocv_module_disable_(python2)
endif()
if(__disable_python3)
  ocv_module_disable_(python3)
endif()
if(__disable_python2 AND __disable_python3)
  return()
endif()

add_subdirectory(python2)
add_subdirectory(python3)
