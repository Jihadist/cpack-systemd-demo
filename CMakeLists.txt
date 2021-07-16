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

# Configure the service file so that it knows where the
# binary was installed.
set(SERVICE_NAME ${PROJECT_NAME}.service)
set(EXE_PATH ${CPACK_PACKAGING_INSTALL_PREFIX}/bin/${PROJECT_NAME})
set(SERVICE_IN ${SERVICE_NAME}.in)
set(SERVICE_OUT ${CMAKE_CURRENT_BINARY_DIR}/${SERVICE_NAME})
configure_file(${SERVICE_IN} ${SERVICE_OUT} @ONLY)

# Install the service file in the share directory. The post
# install RPM script will handle placing the file where
# it's supposed to go.
install(FILES ${SERVICE_OUT} DESTINATION share)

# Configure the scripts that are executed by the DEB.
# This could be done without going through the rigmarole of
# using configures, but I don't want to hard-code the name
# of the service into each file.
#
# Each script has the option of performing verbose logging
# Switch this variable to TRUE to enable those logs.
# I found this helpful while troubeshooting package upgrades.
set(VERBOSE_LOGGING FALSE)
set(POSTINST_IN ${CMAKE_CURRENT_LIST_DIR}/postinst.in)
set(POSTINST_OUT ${CMAKE_CURRENT_BINARY_DIR}/postinst)
configure_file(${POSTINST_IN} ${POSTINST_OUT} @ONLY)
set(POSTRM_IN ${CMAKE_CURRENT_LIST_DIR}/postrm.in)
set(POSTRM_OUT ${CMAKE_CURRENT_BINARY_DIR}/postrm)
configure_file(${POSTRM_IN} ${POSTRM_OUT} @ONLY)
set(PRERM_IN ${CMAKE_CURRENT_LIST_DIR}/prerm.in)
set(PRERM_OUT ${CMAKE_CURRENT_BINARY_DIR}/prerm)
configure_file(${PRERM_IN} ${PRERM_OUT} @ONLY)


set(CPACK_PACKAGE_FILE_NAME ${PROJECT_NAME}-${PROJECT_VERSION})
set(CPACK_PACKAGE_VERSION_MAJOR ${PROJECT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PROJECT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PROJECT_VERSION_PATCH})
set(CPACK_GENERATOR "DEB")
set(CPACK_PACKAGE_NAME ${PROJECT_NAME})
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "MyCompany")
set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
set(CPACK_PACKAGE_RELEASE 1)
set(CPACK_DEBIAN_PACKAGE_CONTROL_EXTRA
    "${POSTINST_OUT}" "${POSTRM_OUT}" "${PRERM_OUT}")
set(CPACK_DEB_POST_INSTALL_SCRIPT_FILE ${POSTINST_OUT})
set(CPACK_DEB_POST_UNINSTALL_SCRIPT_FILE ${POSTUN_OUT})
set(CPACK_DEB_PRE_UNINSTALL_SCRIPT_FILE ${PREUN_OUT})
set(CPACK_DEB_PACKAGE_REQUIRES_POSTUN "systemd")
set(CPACK_DEB_PACKAGE_REQUIRES_PREUN "systemd")
set(CPACK_DEB_PACKAGE_REQUIRES_POST "systemd")
include(CPack)
