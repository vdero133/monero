cmake_minimum_required(VERSION 3.5)
project(monero CXX C)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_BINARY_DIR})
list(APPEND CMAKE_PREFIX_PATH ${CMAKE_BINARY_DIR})


if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
    message(STATUS "Downloading conan.cmake from https://github.com/conan-io/cmake-conan")
    file(DOWNLOAD "https://raw.githubusercontent.com/conan-io/cmake-conan/v0.16.1/conan.cmake"
            "${CMAKE_BINARY_DIR}/conan.cmake"
            EXPECTED_HASH SHA256=396e16d0f5eabdc6a14afddbcfff62a54a7ee75c6da23f32f7a31bc85db23484
            TLS_VERIFY ON)
endif()

include(${CMAKE_BINARY_DIR}/conan.cmake)

conan_cmake_configure(REQUIRES
        boost/1.72.0
        asio/1.18.2
        libbacktrace/cci.20210118
        openssl/1.1.1k
        zeromq/4.3.4
        libsodium/1.0.18
        libunwind/1.5.0
        lmdb/0.9.29
        GENERATORS cmake_find_package
)

conan_cmake_autodetect(settings)

conan_cmake_install(PATH_OR_REFERENCE .
        BUILD missing
        REMOTE conan-center
        SETTINGS ${settings})

include(cmake/TargetArch.cmake)
include(FindLMDB)
include(FindZeroMQ)
find_package(PkgConfig)
find_package(Boost)
find_package(Boost)
find_package(Backtrace)
find_package(OpenSSL)
add_subdirectory(external/easylogging++)
add_subdirectory(external/randomx)
target_include_directories(randomx PUBLIC external/randomx/src/)

add_subdirectory(contrib)
add_subdirectory(src)