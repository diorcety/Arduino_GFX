cmake_minimum_required(VERSION 3.10)
project(Arduino_GFX C CXX)

# 1. Set C++ Standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 2. Collect all source files recursively from the src directory
file(GLOB_RECURSE LIB_SOURCES 
    "src/*.cpp" 
    "src/*.c"
)

# 3. Define the static library
add_library(Arduino_GFX STATIC ${LIB_SOURCES})

# 4. Set include directories
# INTERFACE ensures that projects linking to this library also get these include paths
target_include_directories(Arduino_GFX 
    PUBLIC 
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
        $<INSTALL_INTERFACE:include>
)

# 5. Handle Arduino Dependencies
# Note: Arduino_GFX depends on Arduino.h, SPI.h, and Wire.h.
# If you are building outside of an Arduino IDE environment (e.g., for ESP-IDF or a custom toolchain),
# you must ensure those headers are in your include path.
# Example:
# target_link_libraries(Arduino_GFX PUBLIC YourArduinoCoreLibrary)

# 6. Optional: Installation rules
install(TARGETS Arduino_GFX DESTINATION lib)
install(DIRECTORY src/ DESTINATION include FILES_MATCHING PATTERN "*.h" PATTERN "*.hpp")
