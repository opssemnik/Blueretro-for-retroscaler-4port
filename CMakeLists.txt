# The following lines of boilerplate have to be in your project's
# CMakeLists in this exact order for cmake to work correctly
cmake_minimum_required(VERSION 3.5)

include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project("BlueRetro$ENV{BR_HW}$ENV{BR_SYS}")

# Full flash image (bootloader, partition table, OTA data & app) to be flashed at 0x0,
# e.g. with NodeMCU flasher default settings.
add_custom_target(full_image ALL
    COMMAND ${ESPTOOLPY} merge_bin -o ${CMAKE_PROJECT_NAME}_full.bin -t 0x0 @flash_args
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    DEPENDS gen_project_binary bootloader partition_table_bin blank_ota_data
    COMMENT "Generating ${CMAKE_PROJECT_NAME}_full.bin (flash at 0x0)"
    VERBATIM
    )
