cmake_minimum_required(VERSION 3.5.1)
project(breakpad_common)

set(BREAKPAD_COMMON_LIBRARY_SOURCES
  basictypes.h
  byte_cursor.h
  convert_UTF.cc
  convert_UTF.h
  dwarf_cfi_to_module.cc
  dwarf_cfi_to_module.h
  dwarf_cu_to_module.cc
  dwarf_cu_to_module.h
  dwarf_line_to_module.cc
  dwarf_line_to_module.h
  dwarf_range_list_handler.cc
  dwarf_range_list_handler.h
  language.cc
  language.h
  long_string_dictionary.cc
  long_string_dictionary.h
  macros.h
  md5.cc
  md5.h
  memory_allocator.h
  memory_range.h
  minidump_type_helper.h
  module.cc
  module.h
  path_helper.cc
  path_helper.h
  scoped_ptr.h
  simple_string_dictionary.cc
  simple_string_dictionary.h
  stabs_reader.cc
  stabs_reader.h
  stabs_to_module.cc
  stabs_to_module.h
  stdio_wrapper.h
  string_conversion.cc
  string_conversion.h
  symbol_data.h
  test_assembler.cc
  test_assembler.h
  unordered.h
  using_std_string.h
  dwarf/bytereader-inl.h
  dwarf/bytereader.cc
  dwarf/bytereader.h
  dwarf/cfi_assembler.cc
  dwarf/cfi_assembler.h
  dwarf/dwarf2diehandler.cc
  dwarf/dwarf2diehandler.h
  dwarf/dwarf2enums.h
  dwarf/dwarf2reader.cc
  dwarf/dwarf2reader.h
  dwarf/dwarf2reader_test_common.h
  dwarf/elf_reader.cc
  dwarf/elf_reader.h
  dwarf/functioninfo.cc
  dwarf/functioninfo.h
  dwarf/line_state_machine.h
)

if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
  list(APPEND BREAKPAD_COMMON_LIBRARY_SOURCES
    linux/breakpad_getcontext.h
    linux/breakpad_getcontext.S
    linux/crc32.cc
    linux/crc32.h
    linux/dump_symbols.cc
    linux/dump_symbols.h
    linux/eintr_wrapper.h
    linux/elf_core_dump.cc
    linux/elf_core_dump.h
    linux/elf_gnu_compat.h
    linux/elf_symbols_to_module.cc
    linux/elf_symbols_to_module.h
    linux/elfutils-inl.h
    linux/elfutils.cc
    linux/elfutils.h
    linux/file_id.cc
    linux/file_id.h
    linux/google_crashdump_uploader.cc
    linux/google_crashdump_uploader.h
    linux/guid_creator.cc
    linux/guid_creator.h
    linux/http_upload.cc
    linux/http_upload.h
    linux/ignore_ret.h
    linux/libcurl_wrapper.cc
    linux/libcurl_wrapper.h
    linux/linux_libc_support.cc
    linux/linux_libc_support.h
    linux/memory_mapped_file.cc
    linux/memory_mapped_file.h
    linux/safe_readlink.cc
    linux/safe_readlink.h
    linux/symbol_collector_client.cc
    linux/symbol_collector_client.h
    linux/symbol_upload.cc
    linux/symbol_upload.h
    linux/synth_elf.cc
    linux/synth_elf.h
    linux/ucontext_constants.h
  )
elseif(${CMAKE_SYSTEM_NAME} STREQUAL "Windows")
  list(APPEND BREAKPAD_COMMON_LIBRARY_SOURCES
    windows/dia_util.cc
    windows/dia_util.h
    windows/guid_string.cc
    windows/guid_string.h
    windows/http_upload.cc
    windows/http_upload.h
    windows/module_info.h
    windows/omap.cc
    windows/omap.h
    windows/omap_internal.h
    windows/pdb_source_line_writer.cc
    windows/pdb_source_line_writer.h
    windows/pe_source_line_writer.cc
    windows/pe_source_line_writer.h
    windows/pe_util.cc
    windows/pe_util.h
    windows/string_utils-inl.h
    windows/string_utils.cc
    windows/symbol_collector_client.cc
    windows/symbol_collector_client.h
  )
endif()

add_library(breakpad_common STATIC ${BREAKPAD_COMMON_LIBRARY_SOURCES})

target_include_directories(breakpad_common
  PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}/..
)

if (${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
  find_package(Threads REQUIRED)
  target_link_libraries(breakpad_common
    PRIVATE
    ${CMAKE_THREAD_LIBS_INIT}
  )

  target_compile_definitions(breakpad_common
    PRIVATE
    HAVE_A_OUT)
endif()

if (${CMAKE_SYSTEM_NAME} STREQUAL "Android")
  target_include_directories(breakpad_common
    PRIVATE
    android/include
  )
endif()
