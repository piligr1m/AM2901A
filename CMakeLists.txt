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
	find_package (Threads)
	add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/tests)
	add_subdirectory(third-party/gtest)
	target_link_libraries(${CMAKE_CURRENT_SOURCE_DIR}/third-party/gtest ${CMAKE_THREAD_LIBS_INIT})
endif()

if (COVERAGE)
    target_compile_options(tests PRIVATE --coverage)
    target_link_libraries(tests PRIVATE --coverage)
endif()

install(TARGETS asm_am2901a DESTINATION bin)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/CHANGELOG.md DESTINATION .)

include(CPackConfig.cmake)
