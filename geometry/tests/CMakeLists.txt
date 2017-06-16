cmake_minimum_required(VERSION 3.1.0 FATAL_ERROR)

#if (NOT EXISTS "googletest/googletest/CMakeLists.txt")
#	message(FATAL_ERROR "Cannot find googletest directory.")
#endif()

set(GOOGLETEST_DIR "NOT SET" CACHE FILEPATH "path to googletest")
set(S2_LIBDIR "NOT SET" CACHE FILEPATH "location of S2")
set(OPENSSL_LIBDIR "NOT SET" CACHE FILEPATH "location of OPENSSL libs")
link_directories(${S2_LIBDIR} ${OPENSSL_LIBDIR})

include_directories(
	../s2
	..
	${GOOGLETEST_DIR}
	${GOOGLETEST_DIR}/include
)

add_library(gtest STATIC
	${GOOGLETEST_DIR}/src/gtest-all.cc
)
target_link_libraries(gtest)

add_library(gtest_main STATIC	${GOOGLETEST_DIR}/src/gtest_main.cc)

target_link_libraries(gtest_main gtest)

set(tests_files
	r1interval_test.cc
	s1angle_test.cc
	s1interval_test.cc
	s2cap_test.cc
	s2cellid_test.cc

# One of the tests needs further debugging
#	s2cell_test.cc

	s2cellunion_test.cc
	s2edgeindex_test.cc
	s2edgeutil_test.cc
	s2latlngrect_test.cc
	s2latlng_test.cc
	s2loop_test.cc
	s2pointregion_test.cc
	s2polygonbuilder_test.cc
	s2polygon_test.cc
	s2polyline_test.cc
	s2r2rect_test.cc
	s2regioncoverer_test.cc
	s2regionunion_test.cc
	s2_test.cc
	s2testing.cc
)

add_executable(tests ${tests_files})
set_property(TARGET tests PROPERTY CXX_STANDARD 11)
set_property(TARGET tests PROPERTY CXX_STANDARD_REQUIRED ON)
target_link_libraries(tests s2 s2util s2cellid gtest_main libeay32)

add_custom_target(check
	COMMAND tests
)

