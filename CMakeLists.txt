cmake_minimum_required(VERSION 3.18)

project("portaudiocpp-playground" C CXX)

SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static")

find_package(PortAudio REQUIRED)

IF(PortAudio_FOUND)
    message(STATUS "YES")
ELSE(PortAudio_FOUND)
    message(STATUS "NO")
ENDIF(PortAudio_FOUND)

add_library(PortAudioCpp STATIC IMPORTED)

set_property(
  TARGET PortAudioCpp APPEND PROPERTY IMPORTED_CONFIGURATIONS RELEASE
)

set_target_properties(PortAudioCpp PROPERTIES
  IMPORTED_LINK_INTERFACE_LANGUAGES_RELEASE "CXX"
  IMPORTED_LOCATION_RELEASE "${PortAudioCpp_DIR}/lib/libportaudiocpp.a"
)

set_property( TARGET PortAudioCpp APPEND PROPERTY INTERFACE_LINK_LIBRARIES portaudio_static)


target_include_directories(PortAudioCpp INTERFACE "${PortAudioCpp_DIR}/include")

add_executable(${PROJECT_NAME}
    "src/main.cpp"
)

target_link_libraries(${PROJECT_NAME} PortAudioCpp)
