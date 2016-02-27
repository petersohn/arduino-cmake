#=============================================================================#
# Author: QueezyTheGreat                                                      #
# Date:   26.04.2011                                                          #
#                                                                             #
# Description: Arduino CMake example                                          #
#                                                                             #
#=============================================================================#

# Initialize hardware options
#
# -------------------------------------------------------------------
# Set arduino hardware path
#
#   default is ${ARDUINO_SDK_PATH}/hardware
#
# set(ARDUINO_HARDWARE_PATH $ENV{HOME}/Documents/Arduino/hardware)

# -------------------------------------------------------------------
# Set arduino hardware vendor
#
#   default is arduino
#
# set(ARDUINO_HARDWARE_VENDOR arduino)

# -------------------------------------------------------------------
# Set arduino platform
#
#   default is AVR
#
# set(ARDUINO_PLATFORM AVR)

set(CMAKE_TOOLCHAIN_FILE cmake/ArduinoToolchain.cmake) # Arduino Toolchain


cmake_minimum_required(VERSION 2.8)
#====================================================================#
#  Setup Project                                                     #
#====================================================================#
project(ArduinoExample C CXX)

print_board_list()
print_programmer_list()

# add libraries to project
link_directories(${ARDUINO_SDK}/libraries)

# add the project directory into build
add_subdirectory(example)
