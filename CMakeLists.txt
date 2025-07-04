cmake_minimum_required(VERSION 3.10)

# Project name and version
project(URLShortner VERSION 1.0)

# Set C++ standard
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Define project structure
set(SRC_DIR "${CMAKE_SOURCE_DIR}/src")
set(INCLUDE_DIR "${CMAKE_SOURCE_DIR}/include")
set(UTILS_SRC_DIR "${SRC_DIR}/utils")
set(ROUTES_SRC_DIR "${SRC_DIR}/routes")
set(AUTH_ROUTES_SRC_DIR "${SRC_DIR}/routes/auth")
set(URL_ROUTES_SRC_DIR "${SRC_DIR}/routes/url")
set(USER_ROUTES_SRC_DIR "${SRC_DIR}/routes/user")
set(AUTH_REQUEST_PARSER_SRC_DIR "${SRC_DIR}/request-parser/auth")
set(DATABASE_DIR "${SRC_DIR}/database")

# Include directories
include_directories(${INCLUDE_DIR})

# Source files
set(SOURCES
    ${SRC_DIR}/server.cpp
    ${SRC_DIR}/response.cpp
    ${AUTH_ROUTES_SRC_DIR}/register.cpp
    ${AUTH_ROUTES_SRC_DIR}/login.cpp
    ${USER_ROUTES_SRC_DIR}/get-all-users.cpp
    ${USER_ROUTES_SRC_DIR}/get-user.cpp
    ${URL_ROUTES_SRC_DIR}/url-shorten.cpp
    ${URL_ROUTES_SRC_DIR}/redirect.cpp
    ${ROUTES_SRC_DIR}/404NotFound.cpp
    ${UTILS_SRC_DIR}/helper.cpp
    ${UTILS_SRC_DIR}/short-url-generator.cpp
    ${AUTH_REQUEST_PARSER_SRC_DIR}/register.parser.cpp
    ${DATABASE_DIR}/user.cpp
)

# Add the executable
add_executable(URLShortner ${SOURCES})

# Set output directory
set_target_properties(URLShortner PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/build"
)

# Compiler warnings (optional)
if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU" OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    target_compile_options(URLShortner PRIVATE -Wall -Wextra -pedantic)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    target_compile_options(URLShortner PRIVATE /W4)
endif()

# Find and link OpenSSL
find_package(OpenSSL REQUIRED)
target_link_libraries(URLShortner PRIVATE OpenSSL::Crypto)

# Display a message indicating OpenSSL linking
message(STATUS "OpenSSL found and linked to the project")