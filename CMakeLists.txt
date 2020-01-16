cmake_minimum_required(VERSION 3.2)

if ($ENV{TRAVIS_BUILD_NUMBER})
 project(node_editor VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
else ($ENV{TRAVIS_BUILD_NUMBER})
 project(node_editor VERSION 0.0.1)
endif ($ENV{TRAVIS_BUILD_NUMBER})


set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)

find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)

find_library(GLFW_LIBRARIES glfw ${LIBRARY_SEARCH_PATH})


set( ADDITIONAL_LIBS 
    -lGL
    -lm
    -lGLU
)


message("'GLEW INC DIR ' is equal to: ${GLEW_INCLUDE_DIR}")
message("'GLEW LIBS DIR ' is equal to: ${GLEW_LIBRARIES}")
message("'ADDITIONAL LIBS  ' are equal to: ${ADDITIONAL_LIBS}")
message("'GLFW LIBS DIR ' is equal to: ${GLFW_LIBRARIES}")

include_directories ("lib")
include_directories(PUBLIC ${OPENGL_INCLUDE_DIR})
include_directories(PUBLIC ${GLEW_INCLUDE_DIR})



link_directories(/usr/lib)

add_executable(${PROJECT_NAME} skinning.c)


target_link_libraries(${PROJECT_NAME} ${OPENGL_LIBRARY} ${GLFW_LIBRARIES}  ${GLEW_LIBRARIES} ${ADDITIONAL_LIBS} )



set_target_properties(${PROJECT_NAME} PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
    COMPILE_OPTIONS "-Wpedantic;-Wall;-Wextra"
)




install (TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT alexisvabel@gmail.com)

set(CPACK_DEBIAN_PACKAGE_SHLIDEPS ON)
include(CPack)


