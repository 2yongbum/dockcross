# Minimum CMake required
cmake_minimum_required(VERSION 3.9)

# Project
project(protobuf-examples)

# Find required protobuf package
find_package(Protobuf REQUIRED)

if(protobuf_VERBOSE)
  message(STATUS "Using Protocol Buffers ${protobuf_VERSION}")
endif()

set(CMAKE_INCLUDE_CURRENT_DIR TRUE)

foreach(example add_person list_people)
  set(${example}_SRCS ${example}.cc)
  set(${example}_PROTOS addressbook.proto)

  #Code Generation
  if(protobuf_MODULE_COMPATIBLE) #Legacy Support
    protobuf_generate_cpp(${example}_PROTO_SRCS ${example}_PROTO_HDRS ${${example}_PROTOS})
    list(APPEND ${example}_SRCS ${${example}_PROTO_SRCS} ${${example}_PROTO_HDRS})
  endif()

  #Executable setup
  set(executable_name ${example}_cpp)
  add_executable(${executable_name} ${${example}_SRCS} ${${example}_PROTOS})
  if(protobuf_MODULE_COMPATIBLE) #Legacy mode
    target_include_directories(${executable_name} PUBLIC ${PROTOBUF_INCLUDE_DIRS})
    target_link_libraries(${executable_name} ${PROTOBUF_LIBRARIES})
  else()
    target_link_libraries(${executable_name} protobuf::libprotobuf)
    protobuf_generate(TARGET ${executable_name})
  endif()

endforeach()

file(COPY /usr/local/lib/libprotobuf.so.28.0.3
    DESTINATION .)

ADD_CUSTOM_TARGET(link_target ALL
    COMMAND ${CMAKE_COMMAND} -E create_symlink libprotobuf.so.28.0.3 libprotobuf.so.28)