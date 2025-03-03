# specify the minimum CMake version required

cmake_minimum_required(VERSION 3.10)  

# defining project name and version, enable c++
project(HttpProxy VERSION 1.0 LANGUAGES XCC)

# set C++ to C++17
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# enable additional warnings
if(MSVC)
    add_compile_options(/W4)
else()
    add_compile_options(-Wall -Wextra -pedantic)
endif()

# specify include dictionaries for project headers
include_directories(${CMAKE_SOURCE_DIR}/include)

# ------------------------------------------------------------------------------
# Dependencies
# ------------------------------------------------------------------------------

# find boost libraries: asio, beast, system, file system
find_package(Boost 1.66 REQUIRED COMPONENTS system filesystem)
if(Boost_FOUND)
    message(STATUS "Boost found: ${Boost_INCLUDE_DIRS}")
    include_directories(${Boost_INCLUDE_DIRS})
endif()

# find gumbo parser
find_package(Gumbo REQUIRED)
if(Gumbo_FOUND)
    message(STATUS "Boost found: ${Boost_INCLUDE_DIRS}")
    include_directories(${Boost_INCLUDE_DIRS})
endif()

# find Gumbo Parser
find_package(Gumbo REQUIRED)
if(Gumbo_FOUND)
    message(STATUS "Gumbo Parser found")
    include_directories(${Gumbo_INCLUDE_DIRS})
endif()

# find spdlog (optional logging library)
find_package(spdlog QUIET)
if(spdlog_FOUND)
    message(STATUS "spdlog found")
    include_directories(${spdlog_INCLUDE_DIRS})
else()
    message(STATUS "spdlog not found; logging functionality will be limited")
endif()

# ------------------------------------------------------------------------------
# main Executable
# ------------------------------------------------------------------------------

# gather all source files from the src directory
file(GLOB SOURCES "src/*.cpp")

# create the executable target
add_executable(http-proxy ${SOURCES})

# link required libraries to the executable target
target_link_libraries(http-proxy PRIVATE Boost::system Boost::filesystem)

if(Gumbo_FOUND)
    target_link_libraries(http-proxy PRIVATE ${Gumbo_LIBRARIES})
endif()

if(spdlog_FOUND)
    target_link_libraries(http-proxy PRIVATE spdlog::spdlog)
endif()

# ------------------------------------------------------------------------------
# unit Testing (using GoogleTest)
# ------------------------------------------------------------------------------

enable_testing()

include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/release-1.12.1.zip
)
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

file(GLOB TEST_SOURCES "tests/*.cpp")

add_executable(http-proxy-tests ${TEST_SOURCES})
target_link_libraries(http-proxy-tests PRIVATE gtest_main Boost::system Boost::filesystem)
if(Gumbo_FOUND)
    target_link_libraries(http-proxy-tests PRIVATE ${Gumbo_LIBRARIES})
endif()
if(spdlog_FOUND)
    target_link_libraries(http-proxy-tests PRIVATE spdlog::spdlog)
endif()

include(GoogleTest)
gtest_discover_tests(http-proxy-tests)