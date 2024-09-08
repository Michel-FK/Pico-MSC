cmake_minimum_required(VERSION 3.13)

include(pico_sdk_import.cmake)

project(pico_msc C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
pico_sdk_init()

add_executable(${PROJECT_NAME})

target_sources(${PROJECT_NAME} PUBLIC
  main.c
  msc_disk.c
  usb_descriptors.c
)

#pico_enable_stdio_usb(${PROJECT_NAME} 1)
pico_enable_stdio_uart(${PROJECT_NAME} 1)

# Make sure TinyUSB can find tusb_config.h
target_include_directories(${PROJECT_NAME} PUBLIC
  ${CMAKE_CURRENT_LIST_DIR}
)

# In addition to pico_stdlib required for common PicoSDK functionality, add
# dependency on tinyusb_device for TinyUSB device support and tinyusb_board for
# the additional board support library used by the example
target_link_libraries(${PROJECT_NAME}
  pico_stdlib
  pico_unique_id
  tinyusb_device
  tinyusb_board
)

# Uncomment this line to enable fix for Errata RP2040-E5 (the fix requires use
# of GPIO 15)
target_compile_definitions(${PROJECT_NAME} PUBLIC
  PICO_RP2040_USB_DEVICE_ENUMERATION_FIX=1
)

pico_add_extra_outputs(${PROJECT_NAME})


# cmake_minimum_required(VERSION 3.13)
# include(pico_sdk_import.cmake)
# project(test_project C CXX ASM)
# set(CMAKE_C_STANDARD 11)
# set(CMAKE_CXX_STANDARD 17)
# pico_sdk_init()
# add_executable(test
#   test.c
# )
# pico_enable_stdio_usb(test 1)
# pico_enable_stdio_uart(test 1)
# pico_add_extra_outputs(test)
# target_link_libraries(test pico_stdlib)
