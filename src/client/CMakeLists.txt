cmake_minimum_required(VERSION 3.5.1)
project(breakpad_client)

if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
  add_library(breakpad_client STATIC)

  target_sources(breakpad_client
    PRIVATE
    minidump_file_writer-inl.h
    minidump_file_writer.cc
    minidump_file_writer.h
    linux/crash_generation/client_info.h
    linux/crash_generation/crash_generation_client.cc
    linux/crash_generation/crash_generation_client.h
    linux/dump_writer_common/mapping_info.h
    linux/dump_writer_common/raw_context_cpu.h
    linux/dump_writer_common/thread_info.cc
    linux/dump_writer_common/thread_info.h
    linux/dump_writer_common/ucontext_reader.cc
    linux/dump_writer_common/ucontext_reader.h
    linux/handler/exception_handler.cc
    linux/handler/exception_handler.h
    linux/handler/microdump_extra_info.h
    linux/handler/minidump_descriptor.cc
    linux/handler/minidump_descriptor.h
    linux/log/log.cc
    linux/log/log.h
    linux/microdump_writer/microdump_writer.cc
    linux/microdump_writer/microdump_writer.h
    linux/minidump_writer/cpu_set.h
    linux/minidump_writer/cpu_set_unittest.cc
    linux/minidump_writer/directory_reader.h
    linux/minidump_writer/line_reader.h
    linux/minidump_writer/linux_core_dumper.cc
    linux/minidump_writer/linux_core_dumper.h
    linux/minidump_writer/linux_dumper.cc
    linux/minidump_writer/linux_dumper.h
    linux/minidump_writer/linux_ptrace_dumper.cc
    linux/minidump_writer/linux_ptrace_dumper.h
    linux/minidump_writer/minidump_writer.cc
    linux/minidump_writer/minidump_writer.h
    linux/minidump_writer/proc_cpuinfo_reader.h
    linux/sender/google_crash_report_sender.cc
  )

  target_link_libraries(breakpad_client
    PRIVATE
    breakpad_common
  )

  target_include_directories(breakpad_client
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/..
  )
elseif(${CMAKE_SYSTEM_NAME} STREQUAL "Windows")
  # On Windows separate parts of breakpad client library are compiled into separate libraries

  # crash generation client library
  add_library(crash_generation_client STATIC)

  target_sources(crash_generation_client
    PRIVATE
    windows/crash_generation/client_info.cc
    windows/crash_generation/client_info.h
    windows/crash_generation/crash_generation_client.cc
    windows/crash_generation/crash_generation_client.h
    windows/crash_generation/minidump_generator.cc
    windows/crash_generation/minidump_generator.h
  )

  target_link_libraries(crash_generation_client
    PRIVATE
    breakpad_common
  )

  target_include_directories(crash_generation_client
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/..
  )

  set_target_properties(crash_generation_client PROPERTIES DEBUG_POSTFIX d)

  # crash generation server library
  add_library(crash_generation_server
    PRIVATE
    windows/crash_generation/client_info.cc
    windows/crash_generation/client_info.h
    windows/crash_generation/crash_generation_server.cc
    windows/crash_generation/crash_generation_server.h
    windows/crash_generation/minidump_generator.cc
    windows/crash_generation/minidump_generator.h
  )

  target_link_libraries(crash_generation_server
    PRIVATE
    breakpad_common
  )

  target_include_directories(crash_generation_server
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/..
  )

  set_target_properties(crash_generation_server PROPERTIES DEBUG_POSTFIX d)

  # exception handler library
  add_library(exception_handler STATIC)

  target_sources(exception_handler
    PRIVATE
    windows/handler/exception_handler.cc
    windows/handler/exception_handler.h
  )

  target_link_libraries(exception_handler
    PRIVATE
    breakpad_common
  )

  target_include_directories(exception_handler
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/..
  )

  set_target_properties(exception_handler PROPERTIES DEBUG_POSTFIX d)
endif()
