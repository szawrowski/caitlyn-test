# CaitlynTest

**CaitlynTest** is lightweight header-only C++ testing library.

#### Supported Platforms

- **Linux**

- **Windows**

- **macOS**

#### Supported Standard

- **C++11** or later

## Features

- **Test Definition**: Define tests using macros like `TEST` for standalone tests and `TEST_F` for fixture-based tests.
- **Dynamic Test Registration**: Tests are dynamically registered using a central registry for easy discovery
  and execution.
- **Assertions**: Includes assertion macros (`ASSERT_TRUE`, `ASSERT_EQ`, etc.) for validating expected behaviors.
- **Detailed Reporting**: Reports detailed information on passed and failed tests, aiding in debugging.

# Installation Guide

## Step 1: Clone the Repository

First, navigate to your project root and create an `external` directory to hold third-party libraries.
Next, clone the **CaitlynTets** repository into this folder.

#### Open your terminal and run the following commands

```shell
mkdir external
cd external
git clone https://github.com/szawrowski/caitlyn-test.git
```

## Step 2: Integrate CaitlynTest into your CMake Project

Add the necessary configurations to the `CMakeLists.txt` file to link the
**CaitlynTest** library to your project.

#### Use the following template as a reference

```cmake
cmake_minimum_required(VERSION 3.10)
project(ProjectName)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Specify the path to external dependencies
list(APPEND CMAKE_PREFIX_PATH "${CMAKE_SOURCE_DIR}/external/caitlyn-test")

# Find and include the Caitlyn package configuration
find_package(Caitlyn CONFIG REQUIRED)

# Create an executable target from main.cpp
add_executable(${PROJECT_NAME} main.cpp)

# Link the executable with Caitlyn library
target_link_libraries(${PROJECT_NAME} PRIVATE CaitlynTets::CaitlynTest)
```

Replace `ProjectName` with the actual name of your project and ensure `main.cpp`
is the source file for your main executable.

## Usage

```c++
#include <Caitlyn/Testing>

using namespace Caitlyn::Test;

TEST(MathTests, TestAddition)
{
    ASSERT_EQ(2 + 3, 5);
}

TEST(MathTests, TestFailure)
{
    ASSERT_EQ_PRINTABLE(2 + 2, 5);
}

template <typename T>
struct MathFixture
{
    T a{};
    T b{};

    MathFixture()
    {
    }

    ~MathFixture()
    {
    }

    void SetValues(const T& x, const T& y)
    {
        a = x;
        b = y;
    }
};

TEST_F(MathFixture<int>, TestFixtureAddition)
{
    SetValues(1, 2);
    ASSERT_EQ(a + b, 3);
}

int main()
{
    return TestRegistry::Instance().RunAll();
}
```

### Output

```text
[==========] Running 3 tests from 2 test cases.
[----------] Global test environment set-up.

[----------] 1 tests from MathFixture<int>
[ RUN      ] MathFixture<int>.TestFixtureAddition
[       OK ] MathFixture<int>.TestFixtureAddition (0 ms)
[----------] 1 tests from MathFixture<int> (0 ms total)

[----------] 2 tests from MathTests
[ RUN      ] MathTests.TestAddition
[       OK ] MathTests.TestAddition (0 ms)
[ RUN      ] MathTests.TestFailure
[     FAIL ] MathTests.TestFailure (0 ms)
[     INFO ] Assertion failed: 2 + 2 == 5 (4 != 5)
[----------] 2 tests from MathTests (0 ms total)

[----------] Global test environment tear-down
[==========] 3 tests from 2 test cases ran. (4 ms total)
[  PASSED  ] 2 tests.
[  FAILED  ] 1 test, listed below:
[  FAILED  ] MathTests.TestFailure

1 FAILED TEST
```

## License

This project is licensed under the GNU General Public License v3.0 (GPL-3.0).
See the [LICENSE](LICENSE) file for details.

### GNU General Public License v3.0 (GPL-3.0)

The GPL-3.0 license ensures that everyone has the freedom to run, study, modify, and share the software.

#### Key points include:

- **Freedom to Use**: Use the software for any purpose.
- **Freedom to Study and Modify**: Access the source code to study and change the software.
- **Freedom to Distribute**: Share both original and modified versions, preserving the same freedoms.

For the complete text of the license, refer to the [LICENSE](LICENSE) file.
