# Copyright 2020 Jacob Hootman. All Rights Reserved.

cmake_minimum_required(VERSION 3.15) # required at the top
include_guard(GLOBAL) # prevent configuring the same file multiple times

set(ACTIVE_TARGET_NAME "") # Add the name of the target between the quotations

# Add the names of the header files in quotations
set(ACTIVE_HEADER_FILES

) 

# Add the names of the sources files in quotations
set(ACTIVE_SOURCE_FILES

)

##################################################################################
####################### DON'T CHANGE THIS SECTION ################################
##################################################################################
if(TARGET "${ACTIVE_TARGET_NAME}")
# If the target has already been added to the list, do nothing
else()
# otherwise, we need to configure the target

string(TOUPPER "${ACTIVE_TARGET_NAME}" ACTIVE_TARGET_NAME_UPPER) # create an upper case version of the target name

list(TRANSFORM ACTIVE_HEADER_FILES PREPEND "include/${ACTIVE_TARGET_NAME}/") # format the header files 
list(TRANSFORM ACTIVE_HEADER_FILES APPEND ".hpp")

list(TRANSFORM ACTIVE_SOURCE_FILES PREPEND "src/") # format the source files
list(TRANSFORM ACTIVE_SOURCE_FILES APPEND ".cpp")

##################################################################################
##################################################################################

option("${ACTIVE_TARGET_NAME_UPPER}_INCLUDE_TESTS" "Option to include ${ACTIVE_TARGET_NAME} tests" OFF) # Option to include the testing framework when configuring

add_subdirectory("vendor") # configure vendor code

add_library("${ACTIVE_TARGET_NAME}" "${ACTIVE_HEADER_FILES}" "${ACTIVE_SOURCE_FILES}") # Change add_library to add_executable if you want to compile an executable

set_target_properties( "${ACTIVE_TARGET_NAME}"
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "lib"
    LIBRARY_OUTPUT_DIRECTORY "lib"
    RUNTIME_OUTPUT_DIRECTORY "bin"
) # Change output directories here

target_include_directories("${ACTIVE_TARGET_NAME}" PUBLIC "include") # Set the include directory

target_compile_definitions("${ACTIVE_TARGET_NAME}" PRIVATE "${ACTIVE_TARGET_NAME_UPPER}_BUILD") # Provide a build macro for detecting we are within this library when referencing header files

if("${BUILD_SHARED_LIBS}") # If the configuration is building shared libraries
# Do nothing
else()
# If the configuration is building static libraries
target_compile_definitions("${ACTIVE_TARGET_NAME}" PUBLIC "${ACTIVE_TARGET_NAME_UPPER}_STATIC") # Provide a build macro indicating we are compiling statically
endif()

# target_link_libraries("${ACTIVE_TARGET_NAME}" PUBLIC "") # Add library dependencies with the following code
message(STATUS "${ACTIVE_TARGET_NAME} configured")

if("${${ACTIVE_TARGET_NAME_UPPER}_INCLUDE_TESTS}")
	add_subdirectory("tests")
endif()
endif()