set(CurrLib MyProject)

## set version number
set (${CurrLib}_VERSION_MAJOR "0" CACHE INTERNAL "")
set (${CurrLib}_VERSION_MINOR "0" CACHE INTERNAL "")
set (${CurrLib}_VERSION_PATCH "1" CACHE INTERNAL "")

# get all source files
file(GLOB ${CurrLib}_SOURCES RELATIVE ${CMAKE_SOURCE_DIR}/${CurrLib} *.c;*.cpp;*.txt)
file(GLOB ${CurrLib}_HEADERS RELATIVE ${CMAKE_SOURCE_DIR}/${CurrLib} *.h;*.hpp;../incl/*.h;../incl/*.hpp;../../incl/*.h;../../incl/*.hpp)

## add include directories
foreach(PATH ${PROJECT_INCLUDES})
    include_directories(${PATH})
endforeach(PATH) 

# build a static library
add_library(${CurrLib} STATIC ${${CurrLib}_SOURCES} ${${CurrLib}_HEADERS})
set_target_properties(${CurrLib} PROPERTIES CLEAN_DIRECT_OUTPUT 1)

# set output name
set_target_properties(${CurrLib} PROPERTIES OUTPUT_NAME "${CurrLib}")
set_target_properties(${CurrLib} PROPERTIES PREFIX "lib")

# copy platform config file
configure_file(${CMAKE_SOURCE_DIR}/../../${CurrLib}/src/${CurrLib}Config.h.in ${CMAKE_BINARY_DIR}/incl/${CurrLib}Config.h)

# cppcheck target in the IDE
if (CPPCHECK_FOUND)
    include (GenerateCppcheck)
    GENERATE_CPPCHECK(SOURCES ${${CurrLib}_SOURCES}
                        TARGET_NAME "CppCheck_${CurrLib}"
                      INCLUDES ${ZPL_INCLUDE_LIST})
endif(CPPCHECK_FOUND)