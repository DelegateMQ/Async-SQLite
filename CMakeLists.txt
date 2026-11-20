# Example CMake command line to create project build files:
#
# cmake -B build .
#
# DelegateMQ is fetched from GitHub at the release tag DMQ_GIT_TAG when CMake
# configures. To build against a local DelegateMQ checkout instead, add
# -DFETCHCONTENT_SOURCE_DIR_DELEGATEMQ=<path>. To configure again without
# network access, add -DFETCHCONTENT_FULLY_DISCONNECTED=ON.

# Specify the minimum CMake version required
cmake_minimum_required(VERSION 3.16)

# Project name and language (C or C++)
project(Async-SQLite VERSION 1.1.0 LANGUAGES C CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# DelegateMQ build options, set before DelegateMQ is added
set(DMQ_ALLOCATOR "OFF")
set(DMQ_UTIL "ON")
set(DMQ_THREAD "DMQ_THREAD_STDLIB")
set(DMQ_SERIALIZE "DMQ_SERIALIZE_NONE")
set(DMQ_TRANSPORT "DMQ_TRANSPORT_NONE")

# DelegateMQ release to build against
set(DMQ_GIT_TAG "v2.4.1" CACHE STRING "DelegateMQ release tag (or branch) to fetch")

include(FetchContent)
FetchContent_Declare(
    delegatemq
    GIT_REPOSITORY https://github.com/DelegateMQ/DelegateMQ.git
    GIT_TAG        ${DMQ_GIT_TAG}
    GIT_SHALLOW    TRUE
)
FetchContent_MakeAvailable(delegatemq)

# SQLite and the async wrapper (SQLiteLib links DelegateMQ::DelegateMQ)
add_subdirectory(SQLite)

# Unit tests
add_subdirectory(UnitTest)

# Example application
file(GLOB SOURCES "*.cpp" "*.h")
add_executable(Async-SQLiteApp ${SOURCES})

target_link_libraries(Async-SQLiteApp PRIVATE
    SQLiteLib
    UnitTestLib
)
