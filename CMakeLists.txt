cmake_minimum_required(VERSION 3.5)

if(NOT DEFINED PROJECT_NAME)
  set(NOT_SUBPROJECT ON)
endif()

project(bzip2 VERSION 1.0.8)

option(BZIP2_BUILD_TESTING "whether to enable tests" ${NOT_SUBPROJECT})
set(BZIP2_DIRECTORY "${CMAKE_CURRENT_LIST_DIR}/bzip2")


add_library(bzip2_library_sources INTERFACE)
target_sources(bzip2_library_sources INTERFACE
     "${BZIP2_DIRECTORY}/blocksort.c"
     "${BZIP2_DIRECTORY}/huffman.c"
     "${BZIP2_DIRECTORY}/crctable.c"
     "${BZIP2_DIRECTORY}/randtable.c"
     "${BZIP2_DIRECTORY}/compress.c"
     "${BZIP2_DIRECTORY}/decompress.c"
     "${BZIP2_DIRECTORY}/bzlib.c"
)
target_include_directories(bzip2_library_sources INTERFACE "${BZIP2_DIRECTORY}")


add_library(bzip2_library "")
target_link_libraries(bzip2_library PRIVATE bzip2_library_sources)
target_include_directories(bzip2_library PUBLIC "${BZIP2_DIRECTORY}")
set_target_properties(bzip2_library PROPERTIES
    OUTPUT_NAME "bz2"
    ARCHIVE_OUTPUT_DIRECTORY "lib"
    LIBRARY_OUTPUT_DIRECTORY "lib"
)


add_executable(bzip2_executable)
target_sources(bzip2_executable PRIVATE "${BZIP2_DIRECTORY}/bzip2.c")
target_link_libraries(bzip2_executable PRIVATE bzip2_library_sources)
set_target_properties(bzip2_executable PROPERTIES
    OUTPUT_NAME "bzip2"
    RUNTIME_OUTPUT_DIRECTORY "bin"
)


if (BZIP2_BUILD_TESTING)
    include(CTest)
    add_subdirectory(test)
endif ()
