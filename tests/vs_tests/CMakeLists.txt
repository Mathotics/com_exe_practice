# Copyright 2020 Jacob Hootman. All Rights Reserved.

set(ACTIVE_TEST_TARGET_NAME "${ACTIVE_TARGET_NAME}") # Enter the name of the target that you want to unit test

 # Additional header files that you want to include
set(ACTIVE_HEADER_FILES)

 # Additional source files that you want to include
set(ACTIVE_SOURCE_FILES
"unittests"
)

###############################################################################################
################ DON'T CHANGE ANY OF THE FOLLOWING ############################################
###############################################################################################
set(ACTIVE_TARGET_NAME "test_${ACTIVE_TEST_TARGET_NAME}") # This is the name of the target we are building for testing

string(TOUPPER "${ACTIVE_TARGET_NAME}" "ACTIVE_TARGET_NAME_UPPER") # Create an uppercase version of the active target name

string(TOUPPER "${ACTIVE_TEST_TARGET_NAME}" "ACTIVE_TEST_TARGET_NAME_UPPER") # Create an uppercase version of the active test target name

# Set the install directory (this is used for finding the library that has the visual studio unit testing library)
set(VC_INSTALL_DIR "C:/Program Files (x86)/Microsoft Visual Studio/2017/Community/VC/Auxiliary/VS" CACHE STRING "Same as $(VCInstallDir) on Visual Studio")

# Set the test files that are required for unit testing
set(TEST_FILES "include/stdafx.h" "include/targetver.h" "src/stdafx.cpp")

# Format the header files
list(TRANSFORM ACTIVE_HEADER_FILES PREPEND "include/${ACTIVE_TARGET_NAME}/")
list(TRANSFORM ACTIVE_HEADER_FILES APPEND ".hpp")

# Format the source files
list(TRANSFORM ACTIVE_SOURCE_FILES PREPEND "src/")
list(TRANSFORM ACTIVE_SOURCE_FILES APPEND ".cpp")

add_library("${ACTIVE_TARGET_NAME}" SHARED "${TEST_FILES}" "${ACTIVE_SOURCE_FILES}" "${ACTIVE_HEADER_FILES}")

set_target_properties( "${ACTIVE_TARGET_NAME}"
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "lib"
    LIBRARY_OUTPUT_DIRECTORY "lib"
    RUNTIME_OUTPUT_DIRECTORY "bin"
)

if(${BUILD_SHARED_LIBS}) # If we are building a shared library
# Do nothing
else() # If we are building a static library
target_compile_definitions("${ACTIVE_TARGET_NAME}" PRIVATE "${ACTIVE_TEST_TARGET_NAME_UPPER}_STATIC") # Add the following macro to the target
endif()

# Add the include folder to the header file list
target_include_directories("${ACTIVE_TARGET_NAME}" PRIVATE "include")

# Add the include folder of the test target to the header file list
target_include_directories("${ACTIVE_TARGET_NAME}" PRIVATE "../../include")

target_link_libraries("${ACTIVE_TARGET_NAME}" PRIVATE "${ACTIVE_TEST_TARGET_NAME}")

###############################################################################################
###############################################################################################
