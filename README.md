# Arduino Cmake Example Project

This is the Cmake project settings for the Arduino platform. You can use this project as an example to develop C++ programs in JetBrains CLion IDE for Arduino and using toolchain from Arduino IDE 1.6+.

This project correctly works and tested in Windows with MinGW, but it should work in linux and mac with standard cmake/make pakages.

Original cmake project is licensed with Mozilla Public License, v. 2.0 http://mozilla.org/MPL/2.0/

Readme from the original project is moved to /README.original.rst. You can find all the authors and contributors there.

## Features

- Custom hardware, vendor and platform register. 
- Board CPU option bind to project board settings like `set(${PROJECT_NAME}_CPU XXX)`.
- Most basic features inherits from [francoiscampbell/arduino-cmake](https://github.com/francoiscampbell/arduino-cmake)

## Usefull links

Current project is forked from: [francoiscampbell/arduino-cmake](https://github.com/francoiscampbell/arduino-cmake)

The original project is: [queezythegreat/arduino-cmake](https://github.com/queezythegreat/arduino-cmake)

and with my patches that fixes some major and minor bugs as long as adds other new features.

Some examples are taken from this article http://habrahabr.ru/post/247017/

## Differencies between this project and original one

1. Added patch "Adding support for SDK 1.5" 
   
   https://github.com/queezythegreat/arduino-cmake/pull/104 
   
   Added support for Arduino IDE 1.5-1.6+.
   
2. Added patch "fixed bug in find_file method" 
   
    https://github.com/queezythegreat/arduino-cmake/pull/109 
   
    Now cmake project does not search for arduino toolchain files in standard windows paths.
   
3. Added patch "Fix CMP0038 warnings on CMake >= 3.0" 
   
   https://github.com/queezythegreat/arduino-cmake/pull/143 
   
   Removed warnings.
   
4. Fixed windows paths to avr-size command. Now we can see the size of the compiled arduino program.
   
5. Fixed parsing of the boards.txt settings that are used for compilation of the boards (that differs from arduino mega).
   
6. All compilation settings are taken from original Arduino IDE. 
   
   But I don't think i've done it the best way so you can try do better if you want.
   
7. Removed any differences between all build flavors (debug, release, etc). This is descructive difference between this fork and original project, that is why you should not try to merge this fork into original project. I make it because i dont have enough experience in finding the best compile options for these different build types. So now it uses original Arduino IDE options.
   
8. Original Readme is saved in README.original.rst

## Installing CLion + Arduino IDE + MinGW

1. Install MinGW (needed for CLion, includes make, g++, cpp)

This is needed only for windows. In different OS'es you should skip this and continue to the next - "2. Install Arduino IDE".

1.1. Installing MinGW 

``` 
http://sourceforge.net/projects/mingw/files/Installer/ 
Tested on version mingw-get-0.6.2-mingw32-beta-20131004-1
```

1.2. Run MinGW Installation Manager

1.3. Choose packages from "Basic Setup" 

``` 
- mingw-developer-toolkit
- mingw32-base
- mingw32-gcc-g++
- msys-base
```

and push "Apply Changes"

1. Install Arduino IDE 1.6 (needed for building the project, includes avr toolchain) 
   
    http://www.arduino.cc/en/Main/OldSoftwareReleases 
   
    Tested on version 1.6.3
   
2. Install and setup JetBrains CLion 1.0

3.1. Install JetBrains CLion 1.0 

``` 
https://www.jetbrains.com/clion/ 
Test on version 1.0
```

3.2. Run CLion

3.3. Setup toolchain 

``` 
Menu settings "File" -> "Settings" 
Choose "Build, Execudion, Deployment" -> "Toolchain" 
Setup 
    - Env: MinGW "c:\mingw"
    - cmake: "bundled cmake"
```

MinGW option only exists in windows.

All is done, you can open project in CLion.

## How to move you existing project from Arduino IDE

For example, you have your own project named Robot with the following files

- /Robot.ino 
  
- /Chassis.cpp
  
- /Chassis.h
  
  that already works in Arduino IDE, and you want to move it to CLion IDE.
  
- Make new project with name arduino-cmake-robot 
  
  Clone this repository: 
  
   git clone {THIS_REPO_URI} arduino-cmake-robot

We do not need origin any more, we will use our own project repository. 

Remove origin: 

``` 
git remote rm origin
```

1. Copy our existing project files Robot 
   
   > Make folder /robot in the root of new project. 
   > 
   > Copy files into this folder. Now we have 3 new files in new project
   > 
   > - /robot/Robot.ino
   > - /robot/Chassis.cpp
   > - /robot/Chassis.h
   
2. Rename Robot.ino into robot.cpp 
   
   > Now our new files and folders structure is like the following:
   > 
   > - /robot/robot.cpp
   > - /robot/Chassis.cpp
   > - /robot/Chassis.h
   
3. Add standard Arduino library 
   
   > Add the following line before the very first line of the /robot/robot.cpp file
   > 
   > ``` c++
   > #include "Arduino.h"
   > ```
   
4. Make file with cmake build settings 
   
   > Copy /example/CMakeLists.txt into /robot/CMakeLists.txt
   
5. Setup CMakeLists.txt for "robot" project (/robot/CMakeLists.txt) 
   
   > Set up the name of the project
   > 
   > ``` 
   > set(PROJECT_NAME robot)
   > ```
   
6. Set up the target platform.
   
   > Example 1. For Arduino Pro (Arduino Pro Mini) it should look like this
   > 
   > ``` 
   > set(${PROJECT_NAME}_BOARD pro)
   > # Using CPU 16MHzatmega328. Cloud be found in boards.txt
   > set(${PROJECT_NAME}_CPU 16MHzatmega328)
   > ```
   > 
   > Example 2. For Arduino UNO
   > 
   > ``` 
   > set(${PROJECT_NAME}_BOARD uno)
   > ```
   > 
   > Set up the name of the file, that was previously with INO extension
   > 
   > ``` 
   > set(${PROJECT_NAME}_SRCS robot.cpp)
   > ```
   > 
   > Set up target COM port, which is connect to the board
   > 
   > ``` 
   > set(${PROJECT_NAME}_PORT COM3)
   > ```
   
7. Set up root folder CMakeLists.txt (/CMakeLists.txt)
   
   > Choose hardware path, vendor and platform.
   > 
   > ``` 
   > # Initialize hardware options
   > #
   > # -------------------------------------------------------------------
   > # Set hardware path
   > #
   > #   default is ${ARDUINO_SDK_PATH}/hardware
   > #
   > set(ARDUINO_HARDWARE_PATH $ENV{HOME}/Documents/Arduino/hardware)
   > 
   > # -------------------------------------------------------------------
   > # Set hardware vendor
   > #
   > #   default is arduino
   > #
   > set(ARDUINO_HARDWARE_VENDOR sparkfun)
   > 
   > # -------------------------------------------------------------------
   > # Set arduino platform
   > #
   > #   default is AVR
   > #
   > # set(ARDUINO_PLATFORM AVR)
   > 
   > set(CMAKE_TOOLCHAIN_FILE cmake/ArduinoToolchain.cmake) # Arduino Toolchain
   > ```
   > 
   > Add project folder, change "example" to "robot" 
   > 
   > ``` 
   > # (commented) add_subdirectory(example) 
   > add_subdirectory(robot)
   > ```
   
8. Open project in CLion IDE and choose build option `robot` (for compilation only) or `robot_upload` (for compilation and upload).
   
   > Build project (CTRL+F9). After that project starts to build and upload to the Arduino board.