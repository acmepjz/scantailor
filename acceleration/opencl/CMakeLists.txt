PROJECT(OpenCL)

REMOVE_DEFINITIONS(-DBUILDING_ACCELERATION)

# We ship Khronos versions of OpenCL headers to work-around issues
# with NVidia's OpenCL headers. Even the Khronos headers weren't
# problem-free. The winning combination was cl2.hpp from OpenCL 2.1
# and the rest of the headers from OpenCL 1.1.
FILE(GLOB khronos_headers khronos/CL/*.h khronos/CL/*.hpp)

SET(
	host_sources
	${khronos_headers}
	OpenCLGrid.h
	OpenCLGaussBlur.cpp OpenCLGaussBlur.h
	OpenCLTextFilterBank.cpp OpenCLTextFilterBank.h
	OpenCLDewarp.cpp OpenCLDewarp.h
	OpenCLAffineTransform.cpp OpenCLAffineTransform.h
	OpenCLSavGolFilter.cpp OpenCLSavGolFilter.h
	Transpose.cpp Transpose.h
	Copy.cpp Copy.h
	RenderPolynomialSurface.cpp RenderPolynomialSurface.h
	BinaryFill.cpp BinaryFill.h
	BinaryRasterOp.cpp BinaryRasterOp.h
	HitMissTransform.cpp HitMissTransform.h
	Utils.cpp Utils.h
)

SET(
	plugin_sources
	OpenCLAcceleratedOperations.cpp OpenCLAcceleratedOperations.h
	OpenCLPlugin.cpp OpenCLPlugin.h
)

SOURCE_GROUP("Sources" FILES ${host_sources} ${plugin_sources})
TRANSLATION_SOURCES(scantailor ${plugin_sources})

FILE(GLOB device_sources device_code/*.cl)
SOURCE_GROUP("Sources\\Device" "${device_sources}")
SOURCE_GROUP("Resources" FILES resources.qrc)

INCLUDE_DIRECTORIES(khronos)
ADD_DEFINITIONS(-DCL_HPP_ENABLE_EXCEPTIONS)
ADD_DEFINITIONS(-DCL_HPP_TARGET_OPENCL_VERSION=110)
ADD_DEFINITIONS(-DCL_HPP_MINIMUM_OPENCL_VERSION=110)

IF(ENABLE_OPENCL)
	QT5_ADD_RESOURCES(resource_sources resources.qrc)
	SOURCE_GROUP("Generated" FILES ${resource_sources})

	ADD_LIBRARY(opencl STATIC ${host_sources} ${device_sources} ${resource_sources})
	TARGET_LINK_LIBRARIES(opencl acceleration Qt5::Core Qt5::Gui ${OpenCL_LIBRARIES})

	ADD_LIBRARY(opencl_plugin MODULE ${plugin_sources} resources.qrc)
	TARGET_LINK_LIBRARIES(opencl_plugin opencl dewarping imageproc)

	# Output to the root of the build directory, where it can be found by QPluginLoader.
	SET_TARGET_PROPERTIES(
		opencl_plugin PROPERTIES
		LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}"
	)

	INSTALL(TARGETS opencl_plugin LIBRARY DESTINATION lib/scantailor)

	ADD_SUBDIRECTORY(tests)
ENDIF()
