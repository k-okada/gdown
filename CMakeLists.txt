cmake_minimum_required(VERSION 2.8.12)
project(gdown)

find_package(catkin REQUIRED COMPONENTS catkin_pip)

## use local virutalenv
# defining current package as a package that should be managed by pip (not catkin - even though we make it usable with workspaces)
catkin_pip_package_virtualenv(${PROJECT_NAME})
# CAREFUL : all projects for test here will share the same workspace. pip might have conflicts...

# # We need to install the project pip dependencies in the devel workspace being created
catkin_pip_requirements(${CMAKE_CURRENT_SOURCE_DIR}/requirements.txt)

add_custom_target(debbuild_${PROJECT_NAME}
  COMMAND bloom-generate rosdebian --os-name ubuntu --os-version xenial --ros-distro kinetic
  COMMAND MAKEFLAGS= fakeroot debian/rules binary
  COMMAND dpkg -c ../ros-kinetic-gdown_0.0.1-0xenial_amd64.deb
  WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
  )


