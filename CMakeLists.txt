# Project name is not mandatory, but you should use it
project(affineTracker_gpu)

# States that CMake required version must be >= 2.6
cmake_minimum_required(VERSION 3.6)

# Appends the cmake/modules path inside the MAKE_MODULE_PATH variable which stores the
# directories of additional CMake modules (eg MacroOutOfSourceBuild.cmake):
############################### OPENCV ################################
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR})
find_package( OpenCV )
INCLUDE_DIRECTORIES(${OpenCV_INCLUDE_DIRS})
LINK_DIRECTORIES(${OpenCV_LIBRARY_DIRS})

#message(STATUS "OpenCV_INCLUDE_DIRS ="  ${OpenCV_INCLUDE_DIRS})
#message(STATUS "OpenCV_LIBRARY_DIRS ="  ${OpenCV_LIBRARY_DIRS})
################################ IPP ################################
#set(IPP_FOUND)
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR})

#IF( NOT IPP_FOUND )

FILE(GLOB IPP_INCLUDE_PATHS_1 "/home/anush/intel/compilers_and_libraries_2018.2.199/linux/ipp/include") 
FILE(GLOB IPP_INCLUDE_PATHS_2 " /home/anush/intel/ipp90legacy_lin/linux/include")

SET(
    IPP_INCLUDE_PATHS
    ${IPP_INCLUDE_PATHS_1}
    ${IPP_INCLUDE_PATHS_2}
)


SET(IPP_INCLUDE_DIR ${IPP_INCLUDE_PATHS_1} ${IPP_INCLUDE_PATHS_2})
SET(IPP_LEGACY_INCLUDE_DIR "/home/anush/intel/ipp90legacy_lin/linux/include")

SET(IPPROOT "${IPP_INCLUDE_DIR}/..")
SET(IPP_PATH "${IPP_INCLUDE_DIR}/../lib/intel64/")


            file(GLOB IPP_HDRS "${IPP_PATH}/../../include")
                set(IPP_FOUND TRUE)

    add_definitions(-DHAVE_IPP)
    include_directories(${IPP_INCLUDE_DIR})
    include_directories(${IPP_LEGACY_INCLUDE_DIR})
link_directories(${IPP_PATH})
link_directories("/home/anush/intel/compilers_and_libraries_2018.2.199/linux/ipp/lib/intel64")
link_directories("/home/anush/intel/ipp90legacy_lin/linux/lib/intel64")
    file(GLOB em64t_files "${IPP_PATH}/../../lib/*em64t*")
    set(IPP_ARCH)
    if(em64t_files)
        set(IPP_ARCH "em64t")
    endif()

    set(A ${CMAKE_STATIC_LIBRARY_PREFIX})
    #set(B ${IPP_ARCH}${CMAKE_STATIC_LIBRARY_SUFFIX})
    set(B ${IPP_ARCH}${CMAKE_SHARED_LIBRARY_SUFFIX})
    set(L)
    set(IPP_LIBS ${A}ippcore${B} ${A}ipps${B}  ${A}ippi${B} ${A}ippi90lgc${B})
    set(IPP_LEGACY_LIBS ${A}ippcore${B} ${A}ipps${B}  ${A}ippi${B})
#                 ${A}svml${B} ${A}imf${B} ${A}iomp5${B}   ${A}irc${B} ${A}pthread${B})

message(STATUS "IPPPATH = "  ${IPP_PATH})
message(STATUS "IPP_LIBS ="  ${IPP_LIBS})
message(STATUS "IPP_LEGACY_LIBS ="  ${IPP_LEGACY_LIBS})
message(STATUS "IPP_LEGACY_INC ="  ${IPP_LEGACY_INCLUDE_DIR})

#ENDIF( NOT IPP_FOUND )

#    set(IPP_LIBS ${A}ippcore${B} ${A}ipps${B}  ${A}ippi${B})
#                 ${A}svml${B} ${A}imf${B} ${A}iomp5${B}   ${A}irc${B} ${A}pthread${B})

INCLUDE_DIRECTORIES(${IPP_LEGACY_INCLUDE_DIR})

message(STATUS "IPP_INCLUDE_PATHS = "  ${IPP_INCLUDE_PATHS})
message(STATUS "IPP_LEGACY_INCLUDE_DIR = "  ${IPP_LEGACY_INCLUDE_DIR})
message(STATUS "IPPPATH = "  ${IPP_PATH})
message(STATUS "IPP_LIBS ="  ${IPP_LIBS})
message(STATUS "IPP_LEGACY_LIBS ="  ${IPP_LEGACY_LIBS})



############################### Setup a subdirectory ################################

add_subdirectory(src)
