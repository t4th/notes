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

## generate files
file.c.in
```C
stuffType stuff = {
    .asd = "@ASD@",
    .asd2 = "@ASD2@"
};
```

```
set(ASD "foo")
set(ASD2 "foo2")

add_library(Asd)

set(in_c_file     "${CMAKE_CURRENT_SOURCE_DIR}/file.c.in"   )
set(out_c_file    "${CMAKE_CURRENT_BINARY_DIR}/file.c"      )

cmake_language(EVAL CODE
    "
    function(Asd_configure_file)
        message(STATUS \"Writing: ${out_c_file}\")
        configure_file(\"${in_c_file}\" \"${out_c_file}\" @ONLY)
    endfunction()
    "
)

cmake_language(DEFER DIRECTORY ${CMAKE_SOURCE_DIR} CALL Asd_configure_file)


target_sources(Asd PRIVATE ${out_c_file})

target_include_directories(Asd
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/include
)
```
