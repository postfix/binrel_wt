if(NOT EXISTS "${CMAKE_SOURCE_DIR}/test/doctest/doctest/doctest.h")
  message(WARNING
    "Unit test targets will not be generated because Doctest was not found. "
    "If you want to build the unit tests, update git submodules first and "
    "then regenerate the project."
  )
  return()
endif()

function(add_unittest TEST_NAME)
  set(TARGET_ID "${TEST_NAME}_test")
  add_brwt_executable(${TARGET_ID} ${ARGN})
  target_link_libraries(${TARGET_ID} PRIVATE brwt)
  target_include_directories(${TARGET_ID} SYSTEM PRIVATE
    "${CMAKE_SOURCE_DIR}/test/doctest/doctest")
  add_test(NAME ${TEST_NAME} COMMAND ${TARGET_ID})

  if(NOT BRWT_BUILD_TESTS)
    set_target_properties(${TARGET_ID} PROPERTIES EXCLUDE_FROM_ALL ON)
  endif()
endfunction()

add_subdirectory("brwt")
