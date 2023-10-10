cmake_minimum_required(VERSION 3.15 FATAL_ERROR)

project(examples CXX)

include(FetchContent)

set(CAF_TAG "0.19.4" CACHE STRING "Set CAF version")

set(BUILD_SHARED_LIBS OFF)
set(CAF_ENABLE_EXAMPLES OFF)
set(CAF_ENABLE_TESTING OFF)

FetchContent_Declare(
  caf
  GIT_REPOSITORY https://github.com/actor-framework/actor-framework.git
  GIT_TAG        ${CAF_TAG}
)

FetchContent_MakeAvailable(caf)

enable_testing()

foreach (target example-1 example-2 example-3 example-4 example-5 example-6 example-7 example-8 example-9)
  add_executable(${target} ${target}.cpp)
  target_compile_features(${target} PRIVATE cxx_std_17)
  target_link_libraries(${target} PRIVATE CAF::core CAF::io CAF::net)
  add_executable(${target}-test ${target}.test.cpp)
  target_link_libraries(${target}-test PRIVATE CAF::core CAF::io CAF::net CAF::test)
  target_compile_features(${target}-test PRIVATE cxx_std_17)
  add_test(NAME ${target}-test COMMAND ${target}-test)
endforeach()
