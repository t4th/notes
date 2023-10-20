# CppUTest
## setup

cmake
```c++
cmake_minimum_required (VERSION 3.8)

project ("RunAllTests")

# c++ include paths
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/CppUTest/include)

# this is to detect CppUTest::CppUTest and CppUTest::CppUTestExt
# defined in cmakelist.txt in this dir
add_subdirectory(CppUTest)

# define runnable
add_executable (RunAllTests "test_runnable.cpp" 
  test_case1.cpp
  test_case2.cpp)

target_compile_features(RunAllTests PRIVATE cxx_std_17)

# define link items for runnable
target_link_libraries(RunAllTests PRIVATE
    CppUTest::CppUTest
    CppUTest::CppUTestExt)
```

## Mock recipes

### normal arguments

```c++
bool someFunc(
    uint16_t normal16,
    int16_t & ref,
    uint8_t * c_array,
    uint32_t normal32,
)
{
    return mock().actualCall("someFunc").
        withUnsignedIntParameter("normal16",normal16).
        withOutputParameter("ref",&ref).
        withOutputParameter("c_array",c_array).
        withUnsignedIntParameter("normal32",normal32).
        returnBoolValue();
}

[...]


mock().expectOneCall("someFunc").
    withUnsignedIntParameter("normal16", 9u).
    withOutputParameterReturning("ref", &ref, sizeof(ref)).
    withOutputParameterReturning("c_array", c_array, sizeof(c_array)).
    withUnsignedIntParameter("normal32", 30).
    andReturnValue(true);
```

### Custom comparator

```c++
struct Frame_t
{
    int a;
    unsigned b;
    char c[2];
};

void sendFrame(Frame_t * frame)
{
  return mock().actualCall("sendFrame").
    withParameterOfType("Frame_t", "frame", frame);
}

[...]

class Frame_t_Comparator : public MockNamedValueComparator
{
public:
    bool isEqual(const void* _left, const void* _right) override
    {
        // Casting here the void pointers to the type to compare
        const Frame_t *left = (const Frame_t *) _left; 
        const Frame_t *right = (const Frame_t *) _right;

        if (left->a == right->a && left->b == right->b)
        {
          return 0 == memcmp(left->c, right->c, 2);
        }
        else
          return false;
    }
    virtual SimpleString valueToString(const void* object)
    {
        return (char *) "string";
    }
};

TEST_GROUP(test_group)
{
  Frame_t_Comparator frame_t_Comparator;
  void setup()
  {
      mock().installComparator("Frame_t", frame_t_Comparator);
      mock().crashOnFailure(false);
  }

  void teardown()
  {
      mock().removeAllComparatorsAndCopiers();
      mock().clear();
  }
};

[...]

TEST(test_group, TestName)
{
  Frame_t expected_frame =
  {
    -123, 0xA, {0xFF,0x0C}
  };

  mock().expectOneCall("sendFrame").
    withParameterOfType("Frame_t", "frame", &expected_frame);
}
```

### Custom copier

```c++
#include <vector>
#include "CppUTest/TestHarness.h"
#include "CppUTestExt/MockSupport.h"

class VectorCopier : public MockNamedValueCopier
{
public:
    virtual void copy(void* out, const void* in)
    {
      std::vector<uint8_t> & left = *(std::vector<uint8_t>*)out;
      std::vector<uint8_t> & right = *(std::vector<uint8_t>*)in;

      left = right;
    }
};

TEST_GROUP(test_group)
{
    VectorCopier copier;
    
    void setup()
    {
        mock().installCopier("ByteVector", copier);
        mock().crashOnFailure(false);
    }
    
    void teardown()
    {
        mock().removeAllComparatorsAndCopiers();
        mock().clear();
    }
};

[...]

TEST(test_group, TestName)
{
    std::vector<uint8_t> responseMockData{
        0x00, 0x00, 0x00, 0x00,
        0x00, 0x01, 0x00, 0x00
    };
    
    mock().expectOneCall("class::send")
        .withUnsignedIntParameter("id", id)
        .withOutputParameterOfTypeReturning("ByteVector", "response", &responseMockData)
        .ignoreOtherParameters()
        .andReturnValue(true);
}

[...]

bool send(uint16_t id, std::vector<uint8_t> & response)
{
return mock().actualCall("class::send").
    withUnsignedIntParameter("id",id).
    withOutputParameterOfType("ByteVector","response",&response).
    returnBoolValue();
}
```

### return const descriptor

```c++
const File::Header & File::GetHeader() const
{
    mock().actualCall("File::GetHeader");

    static FileHeader defaultHeader;
    return *(File::Header *)mock().returnPointerValueOrDefault(&defaultHeader);
}

[...]

TEST(test_group, TestName)
{
    File::FileHeader mockFileHeader =
    {
        "name", 123, 123
    };
    
    mock().expectOneCall("File::GetHeader")
        .ignoreOtherParameters()
        .andReturnValue(&mockFileHeader);
}
```

### store data passed as reference to external vector

Capture data passed by mocked function to external vector.

```c++
TEST(test_group, TestName)
{
    std::vector<uint8_t> externalVector{};
    
    mock().setData("DataOutputVector", &externalVector);
    
    // expect some 0xff command
    mock().expectOneCall("Protocol::SendCommand").
        .ignoreOtherParameters()
        andReturnValue(true);
    
    // expected some other cmd with expected input
    uint8_t expected_data[2] = {1,2};
    
    mock().expectOneCall("Protocol::SendCommand").
        withMemoryBufferParameter("data",expected_data, sizeof(expected_data)).
        withUnsignedIntParameter("len", sizeof(expected_data)).
        andReturnValue(true);
}

[...]

bool SendCommand(uint8_t cmd, const uint8_t & data, size_t len)
{
    // command sending data: store data in buffer
    if (0xff == cmd)
    {
        std::vector<uint8_t> & vector{
            *reinterpret_cast<std::vector<uint8_t>*>(
            mock().getData("DataOutputVector").getObjectPointer()
        };
    )
        for (auto i{0u}; i < len; ++i)
        {
            vector.push_back((&data)[i]);
        }
    }

    return mock().actualCall("Protocol::SendCommand").
        withMemoryBufferParameter("data", &data, len).
        withUnsignedIntParameter("len",len).
        returnBoolValue();
}
```
