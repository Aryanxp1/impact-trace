cmake_minimum_required(VERSION 3.20)
project(impact-trace VERSION 0.2.0 LANGUAGES CXX)

function(impact_trace_warnings target)
    if(MSVC)
        target_compile_options(${target} PRIVATE /W4 /permissive-)
    else()
        target_compile_options(${target} PRIVATE -Wall -Wextra -Wpedantic -Wconversion -Wshadow)
    endif()
endfunction()

add_executable(impact-trace impact_trace.cpp)
target_compile_features(impact-trace PRIVATE cxx_std_20)
impact_trace_warnings(impact-trace)

# Backwards compatibility alias
add_executable(impact-trace ALIAS impact-trace)

include(CTest)
if(BUILD_TESTING)
    foreach(test_name IN ITEMS json_tests dep_tests path_tests ctest_tests cmake_tests impact_tests hardening_tests)
        add_executable(${test_name} tests/${test_name}.cpp)
        target_compile_features(${test_name} PRIVATE cxx_std_20)
        impact_trace_warnings(${test_name})
        add_test(NAME ${test_name} COMMAND ${test_name})
    endforeach()
endif()
