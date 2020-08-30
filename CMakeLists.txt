cmake_minimum_required (VERSION 3.5.1)
set(CMAKE_VERBOSE_MAKEFILE ON)
project (rtsp)

find_package(PkgConfig REQUIRED)
pkg_check_modules(GST REQUIRED gstreamer-1.0>=1.14.4)
pkg_check_modules(GLIB REQUIRED glib-2.0)

add_executable(rtsp-streamer
    rtsp.cpp
)

target_include_directories(rtsp-streamer SYSTEM PRIVATE ${GST_INCLUDE_DIRS} ${GLIB_INCLUDE_DIRS})
target_link_libraries(rtsp-streamer -lgstrtspserver-1.0 ${GST_LIBRARIES} ${GLIB_LIBRARY_DIRS})

set_property(TARGET rtsp-streamer PROPERTY CXX_STANDARD 17)
set_property(TARGET rtsp-streamer PROPERTY C_STANDARD 11)
set_property(TARGET rtsp-streamer PROPERTY CXX_EXTENSIONS ON)
set_property(TARGET rtsp-streamer PROPERTY C_EXTENSIONS ON)

target_compile_options(rtsp-streamer PRIVATE -O3 -flto -s -Wall -Wextra -pedantic -Wfatal-errors -Werror -Wno-missing-field-initializers)
set_target_properties(rtsp-streamer PROPERTIES LINK_FLAGS "-flto -s")

install(TARGETS rtsp-streamer DESTINATION bin)
