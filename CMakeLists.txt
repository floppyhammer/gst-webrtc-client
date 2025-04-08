cmake_minimum_required(VERSION 3.10.2)

project(gst_webrtc_client LANGUAGES C VERSION 0.0.1)

option(USE_LIBSOUP2 "Use libsoup2.4 instead of libsoup3.0" OFF)
set(USE_LIBSOUP2 ON)

find_package(PkgConfig REQUIRED)

pkg_check_modules(GLIB REQUIRED glib-2.0)
pkg_check_modules(GST_SDP REQUIRED gstreamer-sdp-1.0)
pkg_check_modules(GST_WEBRTC REQUIRED gstreamer-webrtc-1.0)
pkg_check_modules(GST REQUIRED gstreamer-plugins-base-1.0)
pkg_check_modules(GST REQUIRED gstreamer-plugins-bad-1.0)

if (USE_LIBSOUP2)
    pkg_check_modules(LIBSOUP REQUIRED libsoup-2.4)
else ()
    pkg_check_modules(LIBSOUP REQUIRED libsoup-3.0)
endif ()

pkg_check_modules(JSONGLIB REQUIRED json-glib-1.0)
pkg_check_modules(GIO REQUIRED gio-2.0)

# Default to PIC code
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

add_executable(webrtc_client main.c)

target_link_libraries(
        webrtc_client
        PRIVATE
        ${GST_LIBRARIES}
        ${GST_SDP_LIBRARIES}
        ${GST_WEBRTC_LIBRARIES}
        ${GLIB_LIBRARIES}
        ${LIBSOUP_LIBRARIES}
        ${JSONGLIB_LIBRARIES}
        ${GIO_LIBRARIES}
)

target_include_directories(
        webrtc_client
        PRIVATE
        ${GLIB_INCLUDE_DIRS}
        ${GST_INCLUDE_DIRS}
        ${LIBSOUP_INCLUDE_DIRS}
        ${JSONGLIB_INCLUDE_DIRS}
        ${GIO_INCLUDE_DIRS}
)
