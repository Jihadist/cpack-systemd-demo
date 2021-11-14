# No idea what the minimum version is. This just happens to be
# what I had installed at the time.
cmake_minimum_required(VERSION 3.16)
project(HelloWorld VERSION 0.1)

# Just because our hello-world is using a few C++11 features...
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Setup and install the hello-world application.
add_executable(${PROJECT_NAME} main.cc)
install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)


set(CPACK_PACKAGE_FILE_NAME ${PROJECT_NAME}-${PROJECT_VERSION})
set(CPACK_PACKAGE_VERSION_MAJOR ${PROJECT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PROJECT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PROJECT_VERSION_PATCH})
set(CPACK_GENERATOR "DEB")
set(CPACK_PACKAGE_NAME ${PROJECT_NAME})
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "MyCompany")
set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
set(CPACK_PACKAGE_RELEASE 1)

include(CPack)
