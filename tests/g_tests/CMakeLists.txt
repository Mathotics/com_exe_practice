# Copyright 2020 Jacob Hootman. All Rights Reserved.

set(ACTIVE_TEST_TARGET_NAME "${ACTIVE_TARGET_NAME}") # Add the target that you're testing in the quotations

set(ACTIVE_HEADER_FILES) # Add header files here

set(ACTIVE_SOURCE_FILES
"unittests") # Add source files here

#######################################################################################
################# DON'T CHANGE ANY OF THE FOLLOWING ###################################
#######################################################################################

set(ACTIVE_TARGET_NAME "gtest_${ACTIVE_TEST_TARGET_NAME}") # Create the name of the target that we're building
string(TOUPPER "${ACTIVE_TARGET_NAME}" "ACTIVE_TARGET_NAME_UPPER") # Create an uppercase version of the ACTIVE_TARGET_NAME
string(TOUPPER "${ACTIVE_TEST_TARGET_NAME}" "ACTIVE_TEST_TARGET_NAME_UPPER") # Create an uppercase equivalent of the ACTIVE_TEST_TARGET_NAME

# Format the header files
list(TRANSFORM ACTIVE_HEADER_FILES PREPEND "include/${ACTIVE_TARGET}/")
list(TRANSFORM ACTIVE_HEADER_FILES APPEND ".hpp")

# Format the source files
list(TRANSFORM ACTIVE_SOURCE_FILES PREPEND "src/")
list(TRANSFORM ACTIVE_SOURCE_FILES APPEND ".cpp")

# Create the executable file
add_executable("${ACTIVE_TARGET_NAME}" "${ACTIVE_HEADER_FILES}" "${ACTIVE_SOURCE_FILES}")

set_target_properties("${ACTIVE_TARGET_NAME}"
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "lib"
    LIBRARY_OUTPUT_DIRECTORY "lib"
    RUNTIME_OUTPUT_DIRECTORY "bin"
)

if(${BUILD_SHARED_LIBS})

else()
target_compile_definitions("${ACTIVE_TARGET_NAME}" PUBLIC "${ACTIVE_TEST_TARGET_NAME_UPPER}_STATIC")
endif()

target_link_libraries("${ACTIVE_TARGET_NAME}" PUBLIC "gtest_main")
target_link_libraries("${ACTIVE_TARGET_NAME}" PUBLIC "${ACTIVE_TEST_TARGET_NAME}")

# Add your tests here
add_test(
	NAME "${ACTIVE_TARGET_NAME}"
	COMMAND "${ACTIVE_TARGET_NAME}"
)

#######################################################################################
#######################################################################################
