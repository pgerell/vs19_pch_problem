cmake_minimum_required (VERSION 3.8)

include(CMakeParseArguments)

function(add_precompiled_header _target _input)

  get_filename_component(_input_we ${_input} NAME_WE)
  set(_PCH_SOURCE_CXX "${_input_we}.cpp")

  set(_pch_cxx_pch "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_CFG_INTDIR}/cxx${_input_we}.pch")

  get_target_property(sources ${_target} SOURCES)
  foreach(_source ${sources})
    if(_source MATCHES \\.cpp$)
      if(_source STREQUAL "${_PCH_SOURCE_CXX}")
        set(_pch_compile_flags "\"/Fp${_pch_cxx_pch}\" \"/Yc${_input}\"")
        set_source_files_properties("${_source}" PROPERTIES OBJECT_OUTPUTS "${_pch_cxx_pch}")
      else()
        set(_pch_compile_flags "\"/Fp${_pch_cxx_pch}\" \"/Yu${_input}\"")
        set_source_files_properties("${_source}" PROPERTIES OBJECT_DEPENDS "${_pch_cxx_pch}")
      endif()

      set_source_files_properties(${_source} PROPERTIES
        COMPILE_FLAGS "${_pch_compile_flags}")

    endif()
  endforeach()

endfunction()

set(_PCH_SOURCE "pchheader.cpp")
set(_PCH_HEADER "pchheader.h")

add_executable (CMakeProject2 
  CMakeProject2.cpp
  CMakeProject2.h
  ${_PCH_HEADER}
  ${_PCH_SOURCE}
)
add_precompiled_header(CMakeProject2 ${_PCH_HEADER})
