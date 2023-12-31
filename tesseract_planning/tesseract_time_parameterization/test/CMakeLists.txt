find_package(GTest REQUIRED)

if(NOT TARGET GTest::GTest)
  add_library(GTest::GTest INTERFACE IMPORTED)
  set_target_properties(GTest::GTest PROPERTIES INTERFACE_INCLUDE_DIRECTORIES "${GTEST_INCLUDE_DIRS}")
  if(${GTEST_LIBRARIES})
    set_target_properties(GTest::GTest PROPERTIES INTERFACE_LINK_LIBRARIES "${GTEST_LIBRARIES}")
  else()
    if(MSVC)
      set_target_properties(GTest::GTest PROPERTIES INTERFACE_LINK_LIBRARIES "gtest.lib")
    else()
      set_target_properties(GTest::GTest PROPERTIES INTERFACE_LINK_LIBRARIES "libgtest.so")
    endif()
  endif()
endif()

if(NOT TARGET GTest::Main)
  add_library(GTest::Main INTERFACE IMPORTED)
  set_target_properties(GTest::Main PROPERTIES INTERFACE_INCLUDE_DIRECTORIES "${GTEST_INCLUDE_DIRS}")
  if(${GTEST_MAIN_LIBRARIES})
    set_target_properties(GTest::Main PROPERTIES INTERFACE_LINK_LIBRARIES "${GTEST_MAIN_LIBRARIES}")
  else()
    if(MSVC)
      set_target_properties(GTest::Main PROPERTIES INTERFACE_LINK_LIBRARIES "gtest_main.lib")
    else()
      set_target_properties(GTest::Main PROPERTIES INTERFACE_LINK_LIBRARIES "libgtest_main.so")
    endif()
  endif()
endif()

include(GoogleTest)

# Iterative Spline Time Parameterization Test
add_executable(${PROJECT_NAME}_iterative_spline iterative_spline_tests.cpp)
target_link_libraries(${PROJECT_NAME}_iterative_spline PRIVATE GTest::GTest GTest::Main ${PROJECT_NAME})
target_compile_options(${PROJECT_NAME}_iterative_spline PRIVATE ${TESSERACT_COMPILE_OPTIONS_PRIVATE})
target_compile_options(${PROJECT_NAME}_iterative_spline PUBLIC ${TESSERACT_COMPILE_OPTIONS_PUBLIC})
target_clang_tidy(${PROJECT_NAME}_iterative_spline ENABLE ${TESSERACT_ENABLE_CLANG_TIDY})
target_cxx_version(${PROJECT_NAME}_iterative_spline PRIVATE VERSION ${TESSERACT_CXX_VERSION})
target_code_coverage(
  ${PROJECT_NAME}_iterative_spline
  ALL
  EXCLUDE ${COVERAGE_EXCLUDE}
  ENABLE ${TESSERACT_ENABLE_CODE_COVERAGE})
add_gtest_discover_tests(${PROJECT_NAME}_iterative_spline)
add_dependencies(${PROJECT_NAME}_iterative_spline ${PROJECT_NAME})
add_dependencies(run_tests ${PROJECT_NAME}_iterative_spline)

# Time Optimal Trajectory Generation Tests
add_executable(${PROJECT_NAME}_time_optimal_trajectory_generation_tests time_optimal_trajectory_generation_tests.cpp)
target_link_libraries(${PROJECT_NAME}_time_optimal_trajectory_generation_tests PRIVATE GTest::GTest GTest::Main
                                                                                       ${PROJECT_NAME})
target_compile_options(${PROJECT_NAME}_time_optimal_trajectory_generation_tests
                       PRIVATE ${TESSERACT_COMPILE_OPTIONS_PRIVATE})
target_compile_options(${PROJECT_NAME}_time_optimal_trajectory_generation_tests
                       PUBLIC ${TESSERACT_COMPILE_OPTIONS_PUBLIC})
target_clang_tidy(${PROJECT_NAME}_time_optimal_trajectory_generation_tests ENABLE ${TESSERACT_ENABLE_CLANG_TIDY})
target_cxx_version(${PROJECT_NAME}_time_optimal_trajectory_generation_tests PRIVATE VERSION ${TESSERACT_CXX_VERSION})
target_code_coverage(
  ${PROJECT_NAME}_time_optimal_trajectory_generation_tests
  ALL
  EXCLUDE ${COVERAGE_EXCLUDE}
  ENABLE ${TESSERACT_ENABLE_CODE_COVERAGE})
add_gtest_discover_tests(${PROJECT_NAME}_time_optimal_trajectory_generation_tests)
add_dependencies(${PROJECT_NAME}_time_optimal_trajectory_generation_tests ${PROJECT_NAME})
add_dependencies(run_tests ${PROJECT_NAME}_time_optimal_trajectory_generation_tests)
