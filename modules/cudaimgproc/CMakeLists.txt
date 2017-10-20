if(IOS OR WINRT OR (NOT HAVE_CUDA AND NOT BUILD_CUDA_STUBS))
  ocv_module_disable(cudaimgproc)
endif()
if (CMAKE_CXX_COMPILER_ID STREQUAL GNU)
  # Create a version script for the GNU ld.
  set(CMAKE_VERBOSE_MAKEFILE ON)
  set(_symbols "OPENCV3.3.0 { global: extern \"C++\" {*;}; *;  };")
  set(_version_script "${CMAKE_CURRENT_BINARY_DIR}/version.script")
  file(WRITE ${_version_script} "${_symbols}\n")

  set(CMAKE_SHARED_LINKER_FLAGS " -Wl,--version-script,\"${_version_script}\"")
endif()
set(the_description "CUDA-accelerated Image Processing")

ocv_warnings_disable(CMAKE_CXX_FLAGS /wd4127 /wd4100 /wd4324 /wd4512 /wd4515 -Wundef -Wmissing-declarations -Wshadow -Wunused-parameter)

ocv_define_module(cudaimgproc opencv_imgproc OPTIONAL opencv_cudev opencv_cudaarithm opencv_cudafilters)
