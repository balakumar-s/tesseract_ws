find_package(GTest REQUIRED)
find_package(tesseract_support REQUIRED)
find_package(tesseract_kinematics REQUIRED)

if(NOT WIN32)
  find_package(tcmalloc REQUIRED)
endif()

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

if(NOT WIN32)
  set(TESSERACT_TCMALLOC_LIB tcmalloc::tcmalloc)
endif()

add_executable(${PROJECT_NAME}_unit tesseract_process_managers_unit.cpp)
target_link_libraries(
  ${PROJECT_NAME}_unit
  PRIVATE GTest::GTest
          GTest::Main
          tesseract::tesseract_support
          tesseract::tesseract_kinematics_opw
          ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME}_unit PUBLIC "$<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/examples>")
target_compile_options(${PROJECT_NAME}_unit PRIVATE ${TESSERACT_COMPILE_OPTIONS_PRIVATE}
                                                    ${TESSERACT_COMPILE_OPTIONS_PUBLIC})
target_clang_tidy(${PROJECT_NAME}_unit ENABLE ${TESSERACT_ENABLE_CLANG_TIDY})
target_cxx_version(${PROJECT_NAME}_unit PRIVATE VERSION ${TESSERACT_CXX_VERSION})
target_code_coverage(
  ${PROJECT_NAME}_unit
  ALL
  EXCLUDE ${COVERAGE_EXCLUDE}
  ENABLE ${TESSERACT_ENABLE_CODE_COVERAGE})
add_gtest_discover_tests(${PROJECT_NAME}_unit)
add_dependencies(${PROJECT_NAME}_unit ${PROJECT_NAME})
add_dependencies(run_tests ${PROJECT_NAME}_unit)

add_executable(${PROJECT_NAME}_fix_state_collision_task_generator_unit fix_state_collision_task_generator_unit.cpp)
target_link_libraries(
  ${PROJECT_NAME}_fix_state_collision_task_generator_unit
  PRIVATE GTest::GTest
          GTest::Main
          tesseract::tesseract_support
          ${PROJECT_NAME}
          ${TESSERACT_TCMALLOC_LIB})
target_include_directories(${PROJECT_NAME}_fix_state_collision_task_generator_unit
                           PUBLIC "$<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/examples>")
target_compile_options(${PROJECT_NAME}_fix_state_collision_task_generator_unit PRIVATE ${TESSERACT_COMPILE_OPTIONS})
target_clang_tidy(${PROJECT_NAME}_fix_state_collision_task_generator_unit ENABLE ${TESSERACT_ENABLE_CLANG_TIDY})
target_cxx_version(${PROJECT_NAME}_fix_state_collision_task_generator_unit PRIVATE VERSION ${TESSERACT_CXX_VERSION})
target_code_coverage(
  ${PROJECT_NAME}_fix_state_collision_task_generator_unit
  ALL
  EXCLUDE ${COVERAGE_EXCLUDE}
  ENABLE ${TESSERACT_ENABLE_CODE_COVERAGE})
add_gtest_discover_tests(${PROJECT_NAME}_fix_state_collision_task_generator_unit)
add_dependencies(run_tests ${PROJECT_NAME}_fix_state_collision_task_generator_unit)

add_executable(${PROJECT_NAME}_graph_taskflow_unit graph_taskflow_unit.cpp)
target_link_libraries(
  ${PROJECT_NAME}_graph_taskflow_unit
  PUBLIC ${PROJECT_NAME}
         GTest::GTest
         GTest::Main
         ${TESSERACT_TCMALLOC_LIB})
target_include_directories(${PROJECT_NAME}_graph_taskflow_unit PRIVATE "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>")
target_cxx_version(${PROJECT_NAME}_graph_taskflow_unit PRIVATE VERSION ${TESSERACT_CXX_VERSION})
target_clang_tidy(${PROJECT_NAME}_graph_taskflow_unit ENABLE ${TESSERACT_ENABLE_CLANG_TIDY})
target_code_coverage(
  ${PROJECT_NAME}_graph_taskflow_unit
  ALL
  EXCLUDE ${COVERAGE_EXCLUDE}
  ENABLE ${TESSERACT_ENABLE_CODE_COVERAGE})
add_gtest_discover_tests(${PROJECT_NAME}_graph_taskflow_unit)
add_dependencies(run_tests ${PROJECT_NAME}_graph_taskflow_unit)

# Serialize Tests
add_executable(${PROJECT_NAME}_serialize_unit serialize_test.cpp)
target_link_libraries(${PROJECT_NAME}_serialize_unit PRIVATE GTest::GTest GTest::Main ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME}_serialize_unit PUBLIC "$<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/examples>")
target_compile_options(${PROJECT_NAME}_serialize_unit PRIVATE ${TESSERACT_COMPILE_OPTIONS_PRIVATE}
                                                              ${TESSERACT_COMPILE_OPTIONS_PUBLIC})
target_clang_tidy(${PROJECT_NAME}_serialize_unit ENABLE ${TESSERACT_ENABLE_CLANG_TIDY})
target_cxx_version(${PROJECT_NAME}_serialize_unit PRIVATE VERSION ${TESSERACT_CXX_VERSION})
target_code_coverage(
  ${PROJECT_NAME}_serialize_unit
  ALL
  EXCLUDE ${COVERAGE_EXCLUDE}
  ENABLE ${TESSERACT_ENABLE_CODE_COVERAGE})
add_gtest_discover_tests(${PROJECT_NAME}_serialize_unit)
add_dependencies(run_tests ${PROJECT_NAME}_serialize_unit)
add_dependencies(${PROJECT_NAME}_serialize_unit ${PROJECT_NAME})
