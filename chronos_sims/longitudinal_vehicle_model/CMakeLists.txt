cmake_minimum_required(VERSION 2.8)

project(longitudinal_vehicle_model)

# You need to specify this on your own
SET(Chrono_DIR "/home/david/MyProjects/cubic_engine/chrono_lib/chrono/install/lib64/cmake")

if(${Chrono_DIR} STREQUAL " ")
    message("=====================================================")
    message("ERROR: ")
    message("You must set the Chrono_DIR variable on your own")
    message("=====================================================")
    return()
endif()

find_package(Qt5Core)
SET(CMAKE_INSTALL_PREFIX "/MyProjects/cubic_engine/chrono_lib/chrono/veh_sim_straight_line/vehicle_straight_line_sim")
LIST(APPEND CMAKE_PREFIX_PATH "${CMAKE_INSTALL_PREFIX}/../../Chrono/install/lib64")
find_package(Chrono
             COMPONENTS Irrlicht
             CONFIG)

if (NOT Chrono_FOUND)
    message("=====================================================")
    message("ERROR: ")
    message("Could not find Chrono or one of its required modules")
    message("=====================================================")
    return()
endif()

message(STATUS ${CHRONO_INCLUDE_DIRS})
include_directories(${CHRONO_INCLUDE_DIRS})
add_executable(${PROJECT_NAME} "demo.cpp")

set_target_properties(${PROJECT_NAME} PROPERTIES
                        COMPILE_FLAGS "${CHRONO_CXX_FLAGS} ${EXTRA_COMPILE_FLAGS}"
                        COMPILE_DEFINITIONS "CHRONO_DATA_DIR=\"${CHRONO_DATA_DIR}\""
                        LINK_FLAGS "${CHRONO_LINKER_FLAGS}")

target_link_libraries(${PROJECT_NAME} ${CHRONO_LIBRARIES}  ChronoEngine
                      ChronoEngine_irrlicht
                      ChronoEngine_vehicle
                      ChronoModels_vehicle)
target_link_libraries(${PROJECT_NAME} Qt5::Core)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
add_DLL_copy_command("${CHRONO_DLLS}")

