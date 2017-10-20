if(IOS OR WINRT OR (NOT HAVE_CUDA AND NOT BUILD_CUDA_STUBS))
  ocv_module_disable(cudaarithm)
endif()
if (CMAKE_CXX_COMPILER_ID STREQUAL GNU)
  # Create a version script for the GNU ld.
  set(CMAKE_VERBOSE_MAKEFILE ON)
  set(_symbols "OPENCV3.3.0 { global: extern \"C++\" {*;}; *;  };")
  set(_version_script "${CMAKE_CURRENT_BINARY_DIR}/version.script")
  file(WRITE ${_version_script} "${_symbols}\n")

  set(CMAKE_SHARED_LINKER_FLAGS " -Wl,--version-script,\"${_version_script}\"")
endif()
set(the_description "CUDA-accelerated Operations on Matrices")

ocv_warnings_disable(CMAKE_CXX_FLAGS /wd4127 /wd4324 /wd4512 -Wundef -Wmissing-declarations -Wshadow)

ocv_add_module(cudaarithm opencv_core OPTIONAL opencv_cudev)

ocv_module_include_directories()
ocv_glob_module_sources()

set(extra_libs "")

if(HAVE_CUBLAS)
  list(APPEND extra_libs ${CUDA_cublas_LIBRARY})
endif()

if(HAVE_CUFFT)
  list(APPEND extra_libs ${CUDA_cufft_LIBRARY})
endif()

ocv_create_module(${extra_libs})

ocv_add_accuracy_tests(DEPENDS_ON opencv_imgproc)
ocv_add_perf_tests(DEPENDS_ON opencv_imgproc)
