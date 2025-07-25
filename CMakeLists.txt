########################################################################
# Build Soapy SDR support module for Sidekiq Devices
########################################################################
cmake_minimum_required(VERSION 2.8.12)
project(SoapySidekiq CXX)

find_package(SoapySDR "0.4.0" NO_MODULE REQUIRED)
if (NOT SoapySDR_FOUND)
    message(FATAL_ERROR "Soapy SDR development files not found...")
endif ()

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR})
find_package(Sidekiq)

if (NOT Sidekiq_FOUND)
    message(FATAL_ERROR "Sidekiq development files not found...")
endif ()
message(STATUS "Sidekiq_INCLUDE_DIRS - ${Sidekiq_INCLUDE_DIRS}")
message(STATUS "Sidekiq_LIBRARIES - ${Sidekiq_LIBRARIES}")

# Find libtirpc (RPC/XDR)
find_package(PkgConfig REQUIRED)
pkg_check_modules(TIRPC REQUIRED libtirpc)

# Include directories
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${Sidekiq_INCLUDE_DIRS}
    ${TIRPC_INCLUDE_DIRS}
)

# Link directories
link_directories(${TIRPC_LIBRARY_DIRS})

# Enable C++11 features
if(CMAKE_COMPILER_IS_GNUCXX)

    # C++11 is required
    include(CheckCXXCompilerFlag)
    CHECK_CXX_COMPILER_FLAG("-std=c++11" HAS_STD_CXX11)
    if(HAS_STD_CXX11)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
    else(HAS_STD_CXX11)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
    endif()

    # Thread support enabled
    list(APPEND Sidekiq_LIBRARIES -pthread)

    # Disable warnings for unused parameters
    add_definitions(-Wno-unused-parameter)

endif(CMAKE_COMPILER_IS_GNUCXX)

if (APPLE)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wc++11-extensions")
endif(APPLE)

set(OTHER_LIBS "" CACHE STRING "Other libraries")

SOAPY_SDR_MODULE_UTIL(
    TARGET SidekiqSupport
    SOURCES
        SoapySidekiq.hpp
        Registation.cpp
        Settings.cpp
        Streaming.cpp
        Sensor.cpp
    LIBRARIES
        ${Sidekiq_LIBRARIES}
        ${OTHER_LIBS}
        ${TIRPC_LIBRARIES}
        /usr/lib/epiq/libusb-1.0.so
        /usr/lib/epiq/libglib-2.0.so
        -lrt
)
