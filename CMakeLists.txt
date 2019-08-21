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
ament_target_dependencies(karto Boost)

install(
  DIRECTORY include/
  DESTINATION include
)
install(
  TARGETS karto
  EXPORT export_open_karto
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
  )

ament_export_interfaces(export_open_karto HAS_LIBRARY_TARGET)
ament_export_include_directories(include)
ament_export_libraries(karto)
ament_export_dependencies(Boost)

ament_package()