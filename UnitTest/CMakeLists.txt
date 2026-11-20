# Unit tests for the async SQLite wrapper
file(GLOB SUBDIR_SOURCES "*.c" "*.cpp")
file(GLOB SUBDIR_HEADERS "*.h")

# Create a library target
add_library(UnitTestLib STATIC ${SUBDIR_SOURCES} ${SUBDIR_HEADERS})

# Include directories for the library
target_include_directories(UnitTestLib PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}")

target_link_libraries(UnitTestLib PUBLIC SQLiteLib)

# Add /bigobj flag for MSVC
if (MSVC)
    target_compile_options(UnitTestLib PRIVATE /bigobj)
endif()
