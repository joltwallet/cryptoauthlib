cmake_minimum_required(VERSION 2.6.4)

if (ESP_PLATFORM)
    set(CRYPTOAUTHLIB_DIR "lib")
    idf_component_register(
        SRC_DIRS
            ${CRYPTOAUTHLIB_DIR}/atcacert
            ${CRYPTOAUTHLIB_DIR}/basic
            ${CRYPTOAUTHLIB_DIR}/crypto
            ${CRYPTOAUTHLIB_DIR}/crypto/hashes
            ${CRYPTOAUTHLIB_DIR}/host
            ${CRYPTOAUTHLIB_DIR}/mbedtls
            ${CRYPTOAUTHLIB_DIR}
        INCLUDE_DIRS
            ${CRYPTOAUTHLIB_DIR}
        REQUIRES
            "mbedtls"
        )
        target_compile_options(${COMPONENT_LIB}
            PRIVATE
                -DESP32
                -DATCA_HAL_I2C
                -DATCA_PRINTF
                -DATCA_USE_RTOS_TIMER
        )
else()
    project (cryptoauthlib)

# Set the current release version
    set(VERSION "2018.10.26")
    set(VERSION_MAJOR 2028)
    set(VERSION_MINOR 10)
    set(VERSION_PATCH 26)

# Build Options
    option(BUILD_TESTS "Create Test Application with library" OFF)
#set(ATCA_PKCS11 ON CACHE INTERNAL "")

    message(STATUS "Building Configuration: ${CMAKE_BUILD_TYPE}")


    if(BUILD_TESTS)
    set(ATCA_BUILD_SHARED_LIBS OFF CACHE INTERNAL "")
    endif(BUILD_TESTS)

    add_subdirectory(lib)

# Tests
    if(BUILD_TESTS)
    add_subdirectory(test)
    endif(BUILD_TESTS)
endif()
