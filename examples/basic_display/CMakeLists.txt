# For more information about build system see
# https://docs.espressif.com/projects/esp-idf/en/latest/api-guides/build-system.html
# The following five lines of boilerplate have to be in your project's
# CMakeLists in this exact order for cmake to work correctly
cmake_minimum_required(VERSION 3.5)

set(EXTRA_COMPONENT_DIRS ${CMAKE_CURRENT_LIST_DIR}/../..)

include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project(tm1637_basic_display)

# Workaround for an ESP8266-RTOS-SDK (v3.4) pthread component bug: the component's
# CMakeLists forces "-u pthread_include_pthread_cond_impl" but the real symbol is
# "pthread_include_pthread_cond_var_impl". Without the correct force flag the
# condition-variable code (which defines pthread_cond_init) is dropped by
# --gc-sections and libstdc++ fails to link. Force the correct symbol here.
target_link_libraries(${CMAKE_PROJECT_NAME}.elf
    "-Wl,--undefined=pthread_include_pthread_cond_var_impl")