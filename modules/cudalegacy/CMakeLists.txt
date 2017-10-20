if(NOT HAVE_CUDA)
  ocv_module_disable(cudalegacy)
endif()
if (CMAKE_CXX_COMPILER_ID STREQUAL GNU)
  # Create a version script for the GNU ld.
  set(CMAKE_VERBOSE_MAKEFILE ON)
  set(_symbols "OPENCV3.3.0 { global: extern \"C++\" {*;}; *;  };")
  set(_version_script "${CMAKE_CURRENT_BINARY_DIR}/version.script")
  file(WRITE ${_version_script} "${_symbols}\n")

  set(CMAKE_SHARED_LINKER_FLAGS " -Wl,--version-script,\"${_version_script}\"")
endif()
set(the_description "CUDA-accelerated Computer Vision (legacy)")

ocv_warnings_disable(CMAKE_CXX_FLAGS /wd4127 /wd4130 /wd4324 /wd4512 /wd4310 -Wundef -Wmissing-declarations -Wuninitialized -Wshadow)

ocv_define_module(cudalegacy opencv_core opencv_video
  OPTIONAL opencv_objdetect opencv_imgproc opencv_calib3d opencv_cudaarithm opencv_cudafilters opencv_cudaimgproc)
