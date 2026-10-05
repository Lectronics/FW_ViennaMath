
cmake_minimum_required(VERSION 3.5)

IF(ESP_PLATFORM)

  # ViennaMath is header-only; expose the repo root so consumers can
  # #include "viennamath/...hpp" the same way the examples/ do.
  idf_component_register(INCLUDE_DIRS ".")

  # The headers use throw/typeid/dynamic_cast, but ESP-IDF disables C++
  # exceptions and RTTI by default. Force them on for anything that
  # includes this component (INTERFACE, since the templates are
  # instantiated in the consumer's translation units, not ours).
  target_compile_options(${COMPONENT_LIB} INTERFACE -fexceptions -frtti)

ELSE()

  PROJECT(viennamath)

  set(VERSION_MAJOR 1)
  set(VERSION_MINOR 0)
  set(VERSION_PATCH 0)
  set(VERSION ${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH})

  #SET(CMAKE_VERBOSE_MAKEFILE ON)   #uncomment this line to increase CMake verbosity

  #specify include directory
  INCLUDE_DIRECTORIES(".")

  # Set release build:
  IF(DEFINED CMAKE_BUILD_TYPE)
   SET (CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE})
  ELSE()
   SET (CMAKE_BUILD_TYPE Release)
  ENDIF()

  # examples:
  ADD_EXECUTABLE(basic                examples/basic.cpp)
  ADD_EXECUTABLE(integration          examples/integration.cpp)
  ADD_EXECUTABLE(latex_output         examples/latex_output.cpp)
  ADD_EXECUTABLE(model_benchmark      examples/model_benchmark.cpp)
  ADD_EXECUTABLE(newton_solve         examples/newton_solve.cpp)
  ADD_EXECUTABLE(traversal            examples/traversal.cpp)
  ADD_EXECUTABLE(substitute           examples/substitute.cpp)
  ADD_EXECUTABLE(vector_expr          examples/vector_expr.cpp)

ENDIF()
