cmake_minimum_required(VERSION 3.5.1)
project(breakpad CXX C ASM)

if(APPLE)
  message(FATAL_ERROR "macOS and iOS are not supported at the moment")
endif()

if(CMAKE_COMPILER_IS_GNUCXX)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wimplicit-fallthrough=0 -Wno-missing-field-initializers \
    -Wno-unused-parameter")

  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wimplicit-fallthrough=0 -Wno-missing-field-initializers \
    -Wno-unused-parameter")
endif()

add_subdirectory(src/common)
add_subdirectory(src/client)

if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
  add_subdirectory(src/third_party/libdisasm)
endif()
