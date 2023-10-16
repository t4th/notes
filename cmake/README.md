# cmake
generate and build from the current dir
```
cmake .
```

## single file
CMakeList.txt
```
cmake_minimum_required (VERSION 3.8)

project("project name")

add_executable( Temp "src/main.cpp")
```

## google test
```
cmake_minimum_required (VERSION 3.8)

set(CMAKE_CXX_STANDARD 17)

project("Temp_test")

add_subdirectory(test/googletest)

add_executable( Temp_test "test/test.cpp" "main.cpp" "main.h" )

target_link_libraries(
  Temp_test
  GTest::gtest_main
  GTest::gmock
)
```
