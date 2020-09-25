# Copyright 2002 Jacob Hootman. All Rights Reserved.

if("${MSVC}") # If we are compiling with visual c++
	add_subdirectory("vs_tests") # add the visual studio tests
endif()

add_subdirectory("g_tests") # add the google tests