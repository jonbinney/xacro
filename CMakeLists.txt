cmake_minimum_required(VERSION 2.8.3)
project(xacro)

find_package(catkin REQUIRED)

catkin_python_setup()

catkin_package(
  CFG_EXTRAS xacro-extras.cmake
)

install(PROGRAMS scripts/xacro DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

# Backwards compatability put xacro.py into share so $(find xacro)/xacro.py will work in launch files           
# devel version                                                                                                 
set(PYTHON_SCRIPT ${xacro_SOURCE_DIR}/scripts/xacro)
configure_file(${catkin_EXTRAS_DIR}/templates/script.py.in
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/xacro.py
  @ONLY)

# backwards compatability for installed file                                                                    
set(PYTHON_SCRIPT ${CMAKE_INSTALL_PREFIX}/${CATKIN_PACKAGE_BIN_DESTINATION}/xacro)
configure_file(${catkin_EXTRAS_DIR}/templates/script.py.in
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_BIN_DESTINATION}/to_be_installed/backwards_compatible_xacro.py
  @ONLY)

# install the backwards compatability script too                                                                 
install(PROGRAMS ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_BIN_DESTINATION}/to_be_installed/backwards_compatible_xacro.py DESTINATION 
  ${CATKIN_PACKAGE_SHARE_DESTINATION} RENAME xacro.py)

## Add folders to be run by python nosetests
catkin_add_nosetests(test)
