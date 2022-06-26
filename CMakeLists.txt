cmake_minimum_required(VERSION 3.13)

include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)
include($ENV{PICO_EXTRAS_PATH}/external/pico_extras_import.cmake)
project(pico_webserver)
pico_sdk_init()

set(PICO_TINYUSB_PATH $ENV{PICO_SDK_PATH}/lib/tinyusb)

# set(LWIP_DIR $ENV{LWIP_PATH})
# set (LWIP_INCLUDE_DIRS
#   ${CMAKE_CURRENT_SOURCE_DIR}
#   ${LWIP_DIR}/src/include
#   )
# include(${LWIP_DIR}/src/Filelists.cmake)

set(LWIP_DIR $ENV{PICO_EXTRAS_PATH}/lib/lwip)
set (LWIP_INCLUDE_DIRS
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${LWIP_DIR}/src/include
  )
add_subdirectory(${LWIP_DIR} lwip)

add_executable(${PROJECT_NAME}
  webserver.c
  tusb_lwip_glue.c
  usb_descriptors.c
  ${PICO_TINYUSB_PATH}/lib/networking/dhserver.c
  ${PICO_TINYUSB_PATH}/lib/networking/rndis_reports.c
  )

target_include_directories(${PROJECT_NAME} PRIVATE
  ${CMAKE_CURRENT_LIST_DIR}
  ${LWIP_DIR}/src/include
  ${PICO_TINYUSB_PATH}/lib/networking
  )
  
pico_enable_stdio_usb(${PROJECT_NAME} 0)
pico_enable_stdio_uart(${PROJECT_NAME} 1)

target_link_libraries(${PROJECT_NAME}
  pico_stdlib
  pico_unique_id
  tinyusb_device
  lwipallapps
  lwipcore)

pico_add_extra_outputs(${PROJECT_NAME})

target_compile_definitions(${PROJECT_NAME} PRIVATE PICO_ENTER_USB_BOOT_ON_EXIT=1)
