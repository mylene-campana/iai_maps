# Copyright (c) 2014 CNRS
# Author: Florent Lamiraux
#
# this file is free software: you can redistribute it
# and/or modify it under the terms of the GNU Lesser General Public
# License as published by the Free Software Foundation, either version
# 3 of the License, or (at your option) any later version.
#
# this file is distributed in the hope that it will be
# useful, but WITHOUT ANY WARRANTY; without even the implied warranty
# of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# General Lesser Public License for more details.  You should have
# received a copy of the GNU Lesser General Public License along with
# this file.  If not, see
# <http://www.gnu.org/licenses/>.

cmake_minimum_required(VERSION 2.6)
include(cmake/portability.cmake)
project(iai_maps)

find_package(catkin REQUIRED COMPONENTS
  xacro robot_state_chain_publisher geometry_msgs rostime sensor_msgs tf)

catkin_package()

# build urdf files using xacro
set(XACRO_FILES
  defs/util_defs
  defs/ias_kitchen/materials
  defs/ias_kitchen/furniture_defs
  defs/ias_kitchen/oven_block
  defs/ias_kitchen/sink_block
  defs/ias_kitchen/small_table
  defs/ias_kitchen/fridge_block
  defs/ias_kitchen/table
  defs/ias_kitchen/cuboid
  defs/ias_kitchen/shopping_block
  defs/ias_kitchen/island_block
  defs/ias_kitchen/staircase
  room/kitchen_area
  room/staircase
  room/lab_macros
  room/room
  room/shopping_area
  room/labyrinth
  room/labyrinth2
  )

SET(XACRO_SRDF_FILES
  room/srdf/kitchen_area
  room/srdf/staircase
  room/srdf/labyrinth
  room/srdf/labyrinth2
  )

foreach(it ${XACRO_FILES})
  config_files(${it}.xacro)
  get_filename_component(basepath ${it} PATH)
  get_filename_component(basename ${it} NAME)
  set (basepath ${CMAKE_BINARY_DIR}/${basepath})

  message(" processing xacro file :${basepath}/${basename}.xacro")
  set(expanded_file "${basepath}/${basename}.urdf")
  message("  produce expanded file :"${expanded_file})
  
  execute_process(
    COMMAND ${_xacro_py} -o ${expanded_file} ${basepath}/${basename}.xacro
    )
  set(URDF_FILES ${URDF_FILES} ${expanded_file})
endforeach(it)

foreach(it ${XACRO_SRDF_FILES})
  config_files(${it}.xacro)
  get_filename_component(basepath ${it} PATH)
  get_filename_component(basename ${it} NAME)
  set (basepath ${CMAKE_BINARY_DIR}/${basepath})

  message(" processing xacro file :${basepath}/${basename}.xacro")
  set(expanded_file "${basepath}/${basename}.srdf")
  message("  produce expanded file :"${expanded_file})

  execute_process(
    COMMAND ${_xacro_py} -o ${expanded_file} ${basepath}/${basename}.xacro
    )
  set(SRDF_FILES ${SRDF_FILES} ${expanded_file})
endforeach(it)

INSTALL(FILES
  ${CMAKE_BINARY_DIR}/room/kitchen_area.urdf
  ${CMAKE_BINARY_DIR}/room/labyrinth.urdf
  ${CMAKE_BINARY_DIR}/room/labyrinth2.urdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/urdf
  )

INSTALL(FILES
  ${CMAKE_BINARY_DIR}/room/srdf/kitchen_area.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/labyrinth.srdf
  ${CMAKE_BINARY_DIR}/room/srdf/labyrinth2.srdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/srdf
  )

INSTALL(FILES
  ${CMAKE_BINARY_DIR}/room/staircase.urdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/urdf
  )

INSTALL(FILES
  ${CMAKE_BINARY_DIR}/room/srdf/staircase.srdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/srdf
  )

INSTALL(FILES
  Media/models/meshes/glasses/__Color_A05_4.png
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/Media/models/meshes/glasses
)

INSTALL(FILES
  Media/materials/textures/ganz_rechts_neben_kuecheninsel.png
  Media/materials/textures/hinter_shopschraenken_an_saeule.png
  Media/materials/textures/wallpaper_ai_unihb.png
  Media/materials/textures/stove.png
  Media/materials/textures/ganz_links_vorn_bei_biolab.png
  Media/materials/textures/wallpaper2.png
  Media/materials/textures/ganz_links_neben_roboterplakat.png
  Media/materials/textures/auf_saeule_bei_shop.png
  Media/materials/textures/wallpaper_project_logos.png
  Media/materials/textures/wallpaper.png
  Media/materials/textures/sink.png
  Media/materials/textures/mittig_neben_roboterplakat.png
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/Media/materials/textures
)

INSTALL(FILES
  rviz_configs/iai_kitchen_only.rviz
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/rviz_configs
  )

INSTALL(FILES
  launch/kitchen_with_state_publisher.launch
  launch/iai_kitchen_in_rviz.launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
  )

INSTALL(FILES
  room/floor.urdf
  room/chair.urdf
  room/set.urdf
  room/flower-vase.urdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/urdf
  )
INSTALL(FILES
  meshes/399.jpg
  meshes/chair.dae
  meshes/high_chair.dae
  meshes/set_wine.dae
  meshes/set_wine.stl
  meshes/flower-vase.dae
  meshes/texture.png
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/meshes
  )
