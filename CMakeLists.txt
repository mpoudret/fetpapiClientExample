cmake_minimum_required(VERSION 3.5)

project(Fetpapi_Client_Example)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_POSITION_INDEPENDENT_CODE TRUE)

find_package(Fetpapi REQUIRED)

add_executable (fetpapiClientExample "")

file (GLOB ALL_CPP_FILES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
file (GLOB ALL_H_FILES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.h)

target_sources(fetpapiClientExample PRIVATE ${ALL_CPP_FILES} ${ALL_H_FILES})

target_include_directories(fetpapiClientExample PRIVATE ${AVRO_INCLUDE_DIR} ${Boost_INCLUDE_DIR})

if (WIN32)
	target_link_libraries (fetpapiClientExample PRIVATE bcrypt.lib)
endif (WIN32)

target_compile_definitions(fetpapiClientExample PRIVATE BOOST_ALL_NO_LIB)
target_link_libraries (fetpapiClientExample PRIVATE ${Boost_SYSTEM_LIBRARY} ${CMAKE_THREAD_LIBS_INIT})
if (WIN32)
	if (EXISTS fetpapiClientExample AND EXISTS ${AVRO_LIBRARY_DEBUG})
		target_link_libraries (fetpapiClientExample PRIVATE optimized ${AVRO_LIBRARY_RELEASE} debug ${AVRO_LIBRARY_DEBUG})
	elseif (EXISTS ${AVRO_LIBRARY_RELEASE})
		target_link_libraries (fetpapiClientExample PRIVATE ${AVRO_LIBRARY_RELEASE})
	elseif (EXISTS ${AVRO_LIBRARY_DEBUG})
		target_link_libraries (fetpapiClientExample PRIVATE ${AVRO_LIBRARY_DEBUG})
	endif ()
else (WIN32)
	target_link_libraries (fetpapiClientExample PRIVATE ${AVRO_LIBRARY_RELEASE})
endif (WIN32)

target_link_libraries(fetpapiClientExample PRIVATE Fetpapi::Fetpapi)
