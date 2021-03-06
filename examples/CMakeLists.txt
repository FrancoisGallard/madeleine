#---------------------------------------------------------------------------
#---------------------------------------------------------------------------*/

#Specify the version being used as well as the language
cmake_minimum_required(VERSION 2.8)

option(BUILD_EXAMPLES "Create and install the examples" OFF)

if(BUILD_EXAMPLES)
	# List of examples
	set(EXAMPLE_LIST "")
    list(APPEND EXAMPLE_LIST "example_00001")

	#Rules to build the examples
	foreach(EXAMPLE_NAME IN LISTS EXAMPLE_LIST)
		set(EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/${EXAMPLE_NAME}.cpp")
		list(APPEND EXAMPLES_SOURCES ${EXAMPLE_SOURCES})

		add_executable(${EXAMPLE_NAME} "${EXAMPLE_SOURCES}")
		target_link_libraries(${EXAMPLE_NAME} ${MADELEINE_LIBRARY})
        target_link_libraries(${EXAMPLE_NAME} ${MADELEINE_EXTERNAL_LIBRARIES})
		install(TARGETS ${EXAMPLE_NAME} DESTINATION doc/${CMAKE_PROJECT_NAME}-${MADELEINE_VERSION}/examples)
	endforeach()

	add_custom_target(examples DEPENDS ${EXAMPLE_LIST})
	add_custom_target(clean-examples COMMAND ${CMAKE_MAKE_PROGRAM} clean WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

	set (EXAMPLES_SOURCES ${EXAMPLES_SOURCES} PARENT_SCOPE)

    # Data files
    add_custom_command(
        TARGET "example_00001" PRE_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different "${CMAKE_CURRENT_SOURCE_DIR}/data/sphere1.stl" "${CMAKE_CURRENT_BINARY_DIR}/data/sphere1.stl"
        COMMAND ${CMAKE_COMMAND} -E copy_if_different "${CMAKE_CURRENT_SOURCE_DIR}/data/sphere2.stl" "${CMAKE_CURRENT_BINARY_DIR}/data/sphere2.stl"
    )

endif()




