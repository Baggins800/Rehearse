cmake_minimum_required(VERSION 3.13)
include(FindPackageHandleStandardArgs)
include(GNUInstallDirs)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
option(REHEARSE_INSTALL_DIR "Rehearse install directory." "/usr/local" )
set(CMAKE_INSTALL_FULL_INCLUDEDIR "${REHEARSE_INSTALL_DIR}/include")
set(CMAKE_INSTALL_FULL_LIBDIR "${REHEARSE_INSTALL_DIR}/lib")

project(Rehearse VERSION ${VERSION} LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}")

find_package(COINOR REQUIRED)

file(GLOB Rehearse_SRC "*.cpp" "*.h")
file(GLOB Rehearse_INC *.h)

add_library(${PROJECT_NAME} SHARED ${Rehearse_SRC})
set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER "${Rehearse_INC}")
set_target_properties(${PROJECT_NAME} PROPERTIES POSITION_INDEPENDENT_CODE ON)

if (COINOR_FOUND)
  target_link_libraries(${PROJECT_NAME} coinutils)
  target_link_libraries(${PROJECT_NAME} clp)
  target_link_libraries(${PROJECT_NAME} cbc)
  target_link_libraries(${PROJECT_NAME} osi)
  target_link_libraries(${PROJECT_NAME} osi-clp)
  target_link_libraries(${PROJECT_NAME} osi-cbc)
  target_include_directories(${PROJECT_NAME} PUBLIC ${COINOR_INCLUDE_DIRS})
endif()

target_include_directories(${PROJECT_NAME} PUBLIC
  "*"
  $<INSTALL_INTERFACE:include>
)
install(TARGETS ${PROJECT_NAME}
    LIBRARY DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_FULL_INCLUDEDIR})


