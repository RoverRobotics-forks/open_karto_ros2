cmake_minimum_required(VERSION 3.5)
project(open_karto)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

find_package(ament_cmake REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)

if(BUILD_SHARED_LIBS)
  add_definitions(-DKARTO_DYNAMIC)
endif()

include_directories(include)
add_library(karto SHARED src/Karto.cpp src/Mapper.cpp)
target_include_directories(karto
  PUBLIC
  $<INSTALL_INTERFACE:include>)

ament_target_dependencies(karto
  Boost)

install(
  DIRECTORY include/
  DESTINATION include
)

install(
  TARGETS karto
  EXPORT export_open_karto
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
  )
ament_export_include_directories(include)
ament_export_libraries(karto)
ament_package()