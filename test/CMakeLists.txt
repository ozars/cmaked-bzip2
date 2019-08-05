get_target_property(BZIP2_EXECUTABLE_OUTPUT_DIRECTORY bzip2_executable RUNTIME_OUTPUT_DIRECTORY)
set(BZIP2_EXECUTABLE_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}/${BZIP2_EXECUTABLE_OUTPUT_DIRECTORY}")
set(BZIP_TESTS_DIRECTORY "${CMAKE_CURRENT_LIST_DIR}/bzip2-tests")

add_test(ctest_build_bzip2_executable
    "${CMAKE_COMMAND}" --build ${CMAKE_BINARY_DIR} --target bzip2_executable)

add_test(bzip2_tests
    ${BZIP_TESTS_DIRECTORY}/run-tests.sh
        --bzip2=${BZIP2_EXECUTABLE_OUTPUT_DIRECTORY}/bzip2
        --tests-dir=${BZIP_TESTS_DIRECTORY}
)

set_tests_properties(bzip2_tests PROPERTIES DEPENDS ctest_build_bzip2_executable)
