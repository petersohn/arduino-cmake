# Set a variable for commands below
set(PROJECT_NAME example)

# Define your project and language
project(${PROJECT_NAME} C CXX)

set(${PROJECT_NAME}_BOARD pro)

# Set CPU
set(${PROJECT_NAME}_CPU 16MHzatmega328)

# Define the source code
set(${PROJECT_NAME}_SRCS example.cpp)

# Define the port for uploading code to the Arduino
set(${PROJECT_NAME}_PORT COM6)

# Reset options
#
# Reset with DTR signal
# set(${PROJECT_NAME}_RESET "")

# Reset with DTR signal
# set(${PROJECT_NAME}_RESET "caterina")

# Command to generate code arduino firmware (.hex file)
generate_arduino_firmware(${PROJECT_NAME})
