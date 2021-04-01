cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

project(am2901a VERSION 0.0.1.0)

include(gtest.cmake)

enable_testing()

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)
SET(COVERAGE OFF CACHE BOOL "Coverage")

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/AM2901A)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/AM2901A_ASM)


if (BUILD_TESTING)
	add_subdirectory(third-party/gtest)
	add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/tests)
endif()

find_package (Threads)
target_link_libraries(gtest ${CMAKE_THREAD_LIBS_INIT})

if (COVERAGE)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/tests)
    target_compile_options(${CMAKE_CURRENT_SOURCE_DIR}/tests PRIVATE --coverage)
    target_link_libraries(${CMAKE_CURRENT_SOURCE_DIR}/tests PRIVATE --coverage)
endif()

install(TARGETS asm_am2901a DESTINATION bin)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/CHANGELOG.md DESTINATION .)

include(CPackConfig.cmake)
