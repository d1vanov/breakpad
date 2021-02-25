cmake_minimum_required(VERSION 3.5.1)
project(breakpad_tools)

if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
  add_executable(dump_syms)

  target_sources(dump_syms
    PRIVATE
    linux/dump_syms/dump_syms.cc
  )

  target_link_libraries(dump_syms
    PRIVATE
    breakpad_common
  )
endif()
