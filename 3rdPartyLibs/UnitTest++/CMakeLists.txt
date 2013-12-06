set(CurrLib UnitTest++)

# get sndlib source and header files
file(GLOB ${CurrLib}_SOURCES RELATIVE ${CMAKE_SOURCE_DIR} *.c;*.cpp;*.txt)
file(GLOB ${CurrLib}_HEADERS RELATIVE ${CMAKE_SOURCE_DIR} *.h)

if(GTCMT_WIN32)
    set(PlatformDir Win32)
else(GTCMT_WIN32)
#    set(PlatformDir Win32)
    set(PlatformDir Posix)
endif(GTCMT_WIN32)
file(GLOB ${CurrLib}_SOURCES_2 RELATIVE ${CMAKE_SOURCE_DIR} ${PlatformDir}/*.cpp)
file(GLOB ${CurrLib}_HEADERS_2 RELATIVE ${CMAKE_SOURCE_DIR} ${PlatformDir}/*.h)

## add include directories
foreach(PATH ${PROJECT_INCLUDES})
    include_directories(${PATH})
endforeach(PATH) 

# build a static library
add_library(${CurrLib} STATIC ${${CurrLib}_SOURCES} ${${CurrLib}_HEADERS} ${${CurrLib}_SOURCES_2} ${${CurrLib}_HEADERS_2})
set_target_properties(${CurrLib} PROPERTIES CLEAN_DIRECT_OUTPUT 1)

# set output name
set_target_properties(${CurrLib} PROPERTIES OUTPUT_NAME "${CurrLib}")
set_target_properties(${CurrLib} PROPERTIES PREFIX "lib")

if(MSVC)
	set_target_properties(${CurrLib} PROPERTIES COMPILE_FLAGS "/W1")
endif(MSVC)