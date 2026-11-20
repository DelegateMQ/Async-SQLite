# SQLite (single-file amalgamation, sqlite3.c) and the async wrapper
# (async_sqlite3.cpp)
file(GLOB SUBDIR_SOURCES "*.c" "*.cpp")
file(GLOB SUBDIR_HEADERS "*.h")

# Create a library target
add_library(SQLiteLib STATIC ${SUBDIR_SOURCES} ${SUBDIR_HEADERS})

# Include directories for the library
target_include_directories(SQLiteLib PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}")

# The wrapper's public header includes DelegateMQ.h
target_link_libraries(SQLiteLib PUBLIC DelegateMQ::DelegateMQ)

# Add /bigobj flag for MSVC
if (MSVC)
    target_compile_options(SQLiteLib PRIVATE /bigobj)
endif()
