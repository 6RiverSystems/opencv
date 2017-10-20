set(the_description "Shape descriptors and matchers")
if (CMAKE_CXX_COMPILER_ID STREQUAL GNU)
    # Create a version script for the GNU ld.
    set(CMAKE_VERBOSE_MAKEFILE ON)
    set(_symbols "OPENCV3.3.0 { global: extern \"C++\" {*;}; *;  };")
    set(_version_script "${CMAKE_CURRENT_BINARY_DIR}/version.script")
    file(WRITE ${_version_script} "${_symbols}\n")

    set(CMAKE_SHARED_LINKER_FLAGS " -Wl,--version-script,\"${_version_script}\"")
endif()
ocv_define_module(shape opencv_core opencv_imgproc opencv_video WRAP python)
