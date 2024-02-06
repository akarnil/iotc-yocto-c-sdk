cmake_minimum_required(VERSION 3.0.2)

project(iotc-demo)

set(IOTC_USE_PAHO OFF CACHE BOOL "")
set(IOTC_TPM_SUPPORT OFF CACHE BOOL "") # NOTE: IOTC_USE_PAHO needs to be set to OFF for this
#add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../../iotc-generic-c-sdk SdkSources)
add_subdirectory(${IOTC_C_SDK_DIR} SdkSources)

include(GNUInstallDirs)

include_directories(PRIVATE ${IOTC_C_LIB_INCLUDE_DIR} ${IOTC_C_SDK_INCLUDE_DIR})
add_executable(iotc-demo main.c)

target_link_libraries(iotc-demo iotc-c-generic-sdk)
target_sources(iotc-demo PUBLIC json_parser.c)

